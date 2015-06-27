---
layout: post
title: "面试题一道：三天打渔两天晒网"
date: 2013-11-11 15:03
comments: true
categories: Octopress
description: "一道面试题而已，没有找到问题在哪里。。。"
keywords: tech, C
---

某天去某公司面试，被要求写一个程序。程序要求如下：

某渔民张三，从2010年初开始打渔。该渔夫连续打三天渔后，要用两天时间晒网。要求写一段程序，判断某一日期当天张三是否要去打渔:

    1. 打渔的话返回1，不打渔返回0；
    2. 要有异常处理。

<!--more-->

俺给出的答案如下：

```c
/***************************************************************
*
* fishDay.c
*
****************************************************************
*
* Description:
*
* There is a fishman who stated fishing job from 2010.01.01. 
* He fishes every three days will have a two days rest to clear
* the net.
*
* This program can tell you if the man is fishing on the day 
* you give.
*
***************************************************************/
#include <stdio.h>
#include <stdlib.h>
#include <assert.h>

#define START_YEAR 2010

#ifndef END_YEAR
#define END_YEAR 9999
#endif

/* Count day number from the beginning of the start year to the 
    giving day */
int countDays(int nYear, int nMonth, int nDay) {
    int monthDays [12] = {
        0, 31, 59, 90,
        120, 151, 181, 212,
        243, 273, 304, 334
        };      /* Count how many days past in this year befor 
                    the giving month */

    return (
        365*(nYear-START_YEAR) 
        + monthDays[nMonth-1] 
        + nDay
        + (runDays(nYear) - runDays(START_YEAR)) 
        - 1     /* Remove the Jan 1st ,2010 */
        );
}

/* Count how many run year past from year 0 */
int runDays(int nYear) {
    return ( nYear/4 - nYear/100 + nYear/400) ; 
}

unsigned int fishDay(int nYear, int nMonth, int nDay) {

    int days;   /* Count how many days are there from the beginning
                    of the start year to the giving day */
    int daysInMonth[12] = {
        29, 31, 28, 31, 30,
        31, 30, 31, 31,
        30, 31, 30, 31
        };      /* Day number in every month */
    
    /* Check the input nYear, nMonth and nDay */
    assert((START_YEAR <= nYear) && (END_YEAR >= nYear));
    assert((1 <= nMonth) && (12 >= nMonth));

    if (2 == nMonth) {
        if ((0 == nYear%400) 
            || ((0 == nYear%4) && (0 != nYear%100))) {
            assert((1 <= nDay) && (daysInMonth[0] >= nDay));
        }
    } else 
        assert((1 <= nDay) && (daysInMonth[nMonth] >= nDay));

    /* Check if the day is a fishing day */
    days = countDays(nYear, nMonth, nDay);

    if((days % 5) <= 2)
        return 1;   /* It's a fishing day */
    else
        return 0;   /* It's a break day */
}

/* Test case
int main() {

    if (fishDay(2010,1,0))
        printf("2010,1,0 is a fishing day.\n");
    else
        printf("2012,1,0 is not a fishing day.\n");

    if (fishDay(2010,1,5))
        printf("2010,1,5 is a fishing day.\n");
    else
        printf("2012,1,5 is not a fishing day.\n");
    
    if (fishDay(2012,1,6))
        printf("2012,1,6 is a fishing day.\n");
    else
        printf("2012,1,6 is not a fishing day.\n");
    
    if (fishDay(2013,1,5))
        printf("2013,1,5 is a fishing day.\n");
    else
        printf("2013,1,5 is not a fishing day.\n");

    return 0;
}
*/
```
后来结果是由于俺的简历不过关，面试官觉得后面的面试我再往后走就走不下去了，所以杯具了……我总觉得这个理由只不过是个借口，估计问题还是出在这段代码本身。

做了几年的QA，代码水平有所退步这个我是承认的。可是我还是没怎么能看出这段代码出什么大问题了。麻烦大家给挑挑刺呗。^_^
