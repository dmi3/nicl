#!/usr/bin/python3
# -*- coding: utf-8 -*-

from calendar import TextCalendar
from datetime import date, timedelta, datetime
import os
import sys
import argparse
import dateparser
import re

META = '\33[90m'
TODAY = '\33[7m'
WEEKEND = '\33[36m'
CEND = '\33[0m'

def color(text, color):
    return color+text+CEND if sys.stdout.isatty() else text

def formatmonth(date, highlight, current=False, weeknumbers=False, w=0):
    theyear = date.year
    themonth = date.month
    firstweeknumber = date.replace(day=1).isocalendar()[1] if weeknumbers else -100
    weekcolumn = ' № ' if weeknumbers else ''
    weekpadding = 3 if weeknumbers else 0
    day = date.day if current else 0
    highlightdays = [x.day for x in highlight if x.year == theyear and x.month == themonth]

    w = max(2, w)
    s = []
    s.append(color(cal.formatmonthname(theyear, themonth, 7 * (w + 1) + weekpadding - 1), META))
    s.append(color(cal.formatweekheader(w).rstrip() + weekcolumn, META))
    for i,week in enumerate(cal.monthdays2calendar(theyear, themonth)):
        s.append(formatweek(week, highlightdays, firstweeknumber + i, day, w).rstrip())
    return s

def formatweek(theweek, highlightdays, weeknumber, day, width):
    weekcolumn = color(" %i" % weeknumber, META) if weeknumber>0 else ''
    return ' '.join(formatday(highlightdays, d, wd, day, width) for (d, wd) in theweek) + weekcolumn

def formatday(highlightdays, day, weekday, today, width):
    if day == 0:
        s = ''
    elif day == today:
        s = color('%2i' % day, TODAY)
    elif day in highlightdays or weekday > 4:
        s = color('%2i' % day, WEEKEND)
    else:
        s = '%2i' % day
    return s.center(width)

def align(lines, pos, length):
    text = "" if len(lines)<=pos else lines[pos]
    printable = re.sub("\33\[\d{1,2}m","", text)
    for i in range(0, length-len(printable)):
        text+=" "
    return text

parser = argparse.ArgumentParser(description='Nicer Calendar for your terminal')
parser.add_argument('date', nargs="*", default=None, help='Human readable date')
parser.add_argument('-3', '--three',  action='store_true', help='Display three months spanning the date.')
parser.add_argument('-w', '--weeknumbers',  action='store_true', help='Display week numbers in the calendar')
parser.add_argument('-s', '--sunday',  action='store_true', help='Display Sunday as the first day of the week')
parser.add_argument('-f', '--file', help='File with dates to highlight')
parser.add_argument('-d', '--format', default='%Y-%m-%d', help='Format of date in file')
args = parser.parse_args()

firstweekday = 6 if args.sunday else 0
dateformat = args.format

highlight = []
if args.file:
    with open(args.file, 'r') as myfile:
        for line in myfile.readlines():
            highlight.append(datetime.strptime(line.strip(), dateformat))

cal = TextCalendar(firstweekday)
today = datetime.today() if len(args.date)==0 else dateparser.parse(' '.join(args.date), settings={'PREFER_DAY_OF_MONTH': 'first'})

this = formatmonth(today, highlight, current=True, weeknumbers=args.weeknumbers)

if not args.three:
    print("\n".join(this))
else:
    l = 24 if args.weeknumbers else 21
    prev = formatmonth(today.replace(day=1) - timedelta(days=1), highlight, weeknumbers=args.weeknumbers)
    neht = formatmonth(today.replace(day=1) + timedelta(days=31), highlight, weeknumbers=args.weeknumbers)
    rows = max(len(prev), len(this), len(neht))

    columns = os.get_terminal_size().columns
    if l*2>=columns:
        print("\n".join(prev))
        print("\n"+"\n".join(this))
        print("\n"+"\n".join(neht))
    elif l*3>=columns:
        for i in range (0, rows):
            print(align(prev, i, l), align(this, i, l))
        print("\n"+"\n".join(neht))
    else:
        for i in range (0, rows):
            print(align(prev, i, l), align(this, i, l), align(neht, i, l))
