---
layout: post
title: USD-BRL exchange rates in Google Sheets and Excel
author: Mírian
description: Exploring sheets possibilities for pulling Central Bank of Brazil data for USD-BRL conversion at a given day.
tags: finance brazil tax exchange-rate usd income irpf brl google-sheets excel power-query
---

Doing taxes can be a challenge. If you receive money in a foreign currency, being a Brazil resident, income tax it is definitively tricky. 

If you receive money from abroad, you need to do some math each month to learn what is the amount you should consider when calculating your taxes. Regardless of the currency your money is in, you need to convert it first to US dollars, then to Brazilian Reais (BRL), and just then you can do your tax calculation. But the USD-BRL conversion needs to use the exchange rate for (follow me if you can!) the last business day of the first fortnight of the month prior to the payment month.

![Woman looking confused, math formulas all around her](https://media.giphy.com/media/WRQBXSCnEFJIuxktnw/giphy.gif)

Here I detail my experiments to create a step by step sheet document (using Google sheets and Excel), to not need to think this through every month.

![Fun-looking blond girl gesturing a lot, subtitle says "getting very excited about numbers and things"](https://media.giphy.com/media/tBn5FJVUdeMarcl3gQ/giphy.gif)

## Google sheets

We have 4 logical steps to find the date. Remember the definition: the exchange rate to be used is for "the last business day of the first fortnight of the month prior to the payment month". Once we have the payment date (that you need to enter), let's follow the steps below:

1. Find the payment month, using a creative [EOMONTH](https://support.google.com/docs/answer/3093044) ("End of Month")
2. Find the month prior to the payment month, also with `EOMONTH`
3. Find the first fortnight of the month prior to the payment month (formally an interval of 14 days, but what is meant here is "first half of the month"), adding 15 days to the date in the previous step
4. Find the last business day of the first fortnight of the month prior to the payment month, using [WEEKDAY](https://support.google.com/docs/answer/3092985) with the second parameter as `2`, starting weeks with Monday and thus having Saturday and Sunday easy to spot with an `IF` clause

Of course all of this can be done in one step only, but it would be harder to follow. Since my purpose here was to keep some clarity (i.e., where do these dates come from?), I chose to show all the steps, despite the last date is the date we need.

![Homer Simpson dressed funny saying "Hmm I understand". I wonder if he did.](https://media.giphy.com/media/xT5LMRBvRxAHZfQh5m/giphy.gif)

An additional note: I haven't considered holidays, so if the last business day of the first fortnight ends up in a holiday this won't work. One example is the Carnival 2021, that was celebrated in Brazil in the Monday, February 14th and Tuesday, February 15th (in fact, Carnival is celebrated since the Friday before, but not so much in a pandemic year, and that's another story anyway). The thing is you can set your own manual date here (which would be February 12th), while I think of better ways.

We have done all this just to find the date. Now we need the exchange rate for it. For income tax purposes in Brazil, we cannot use the beautiful, easy [GOOGLEFINANCE](https://support.google.com/docs/answer/3093281) function to get the rates data, for it needs to be the official exchange rate data provided by the Central Bank of Brazil. This exchange rate is not the last exchange rate of any given day, but an average of several exchange rates during that day, extracted from bidding agents in 4 time windows. In [this page](https://www.bcb.gov.br/en/financialstability/quotations) there's a footnote describing how this is calculated. 

Therefore we need to use the official sources. Fine. To find the rates, here I used [IMPORTDATA](https://support.google.com/docs/answer/3093335) and [QUERY](https://support.google.com/docs/answer/3093343) with the CSV provided by the Bank. At the time I created the first version of these documents, I couldn't find any API to retrieve this data (now [there is](https://dadosabertos.bcb.gov.br/dataset/dolar-americano-usd-todos-os-boletins-diarios/resource/ae69aa94-4194-45a6-8bae-12904af7e176), looking forward to using it shortly!).

Here you can see the [resulting Google sheets document](https://docs.google.com/spreadsheets/d/1GNjj3T8Xui7oRoQkrKOgb3Jx-1ptR16IjtY-gX_73gg/edit#gid=0) ([pt-BR version](https://docs.google.com/spreadsheets/d/1ijILRdL32EJjuhzPYAs3rjAdj83viPhWnZO5BG2Wu_Y/edit?usp=sharing)).

## Microsoft Excel

In Excel, the same steps apply - and some of the functions even have the same name!

Have the payment date entered, and let's follow together the steps to get the date for the exchange rate:

1. Find the payment month, using a creative [EOMONTH](https://support.microsoft.com/en-us/office/eomonth-function-7314ffa1-2bc9-4005-9d66-f49db127d628)
2. Find the month prior to the payment month, also with `EOMONTH`
3. Find the first fortnight of the month prior to the payment month, only by adding 15 days to the date in the previous step
4. Find the last business day of the first fortnight of the month prior to the payment month, using [WEEKDAY](https://support.microsoft.com/en-us/office/weekday-function-60e44483-2ed1-439f-8bd0-e404c190949a) with the second parameter as `2`, starting weeks with Monday and thus having Saturday and Sunday easy to spot with an `IF` clause

In Excel too I haven't considered holidays. Beware of Carnival, November 15th ([Republic Day in Brazil](https://en.wikipedia.org/wiki/Proclamation_of_the_Republic_(Brazil))) and so many others I may be forgetting.

![Warning sign in red](https://media.giphy.com/media/Y5wlazC8lSVuU/giphy.gif)

The dates above can be used to forge an URL to extract CSV data from, that will look like something this: https://ptax.bcb.gov.br/ptax_internet/consultaBoletim.do?method=gerarCSVFechamentoMoedaNoPeriodo&ChkMoeda=61&DATAINI=14/04/2021&DATAFIM=15/04/2021. `DATAINI` means start date, and `DATAFIM` end date.

Now, to import the exchange rates from CSV. Here comes the main difference between Google sheets and Excel. Google sheets provide a very nice `IMPORTDATA` function that works directly in the sheet, while Excel has no such thing. There's all the data importing, scrapping and processing tools that come together with Power Query, so I had to use the UI a bit.

![Mr. Krabs agreeing to something. He looks happy.](https://media.giphy.com/media/l0EtMMARnUBHCzZ3G/giphy.gif)

I find this the easiest way to create the connection: import the CSV using the **Data** tab in Excel, and then change the connection to be a function. You can process the origin CSV manually to remove all columns except the one for the exchange rate, and have only the last row to be shown, or use the function I have created:
