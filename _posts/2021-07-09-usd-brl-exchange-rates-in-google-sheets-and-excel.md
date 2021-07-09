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

