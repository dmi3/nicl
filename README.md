# NiCl 

NiCl - nicer calendar for your terminal. Like [cal](https://linux.die.net/man/1/cal) but nicer:

* Parses natural language dates (human readable) using [dateparser](https://dateparser.readthedocs.io/en/latest/):
    - `nicl next month`
    - `nicl 12 november 2030`
    - `nicl 2012 jan`
    - ...
* Prints colored output if terminal supports it
* Optionally shows week numbers
* Optionally you can pass file with additional dates to highlight:
  - Useful so you can download bank holidays from your local goverment site
  - Highlight only dates important to you!
* Hackable!
  - If don't like colors, format etc is easy to adapt this simple code for your needs
* You can create `alias nicl2=nicl` if you are chemistry geek, and incorrect spelling of nickel chloride bothers you

Wrapper for Python awesome [calendar](https://github.com/python/cpython/blob/3.5/Lib/calendar.py)

## Dependencies

    pip3 install dateparser --user

## Usage

    ➤ nicl -h

    positional arguments:
      date                  Human readable date

    optional arguments:
      -h, --help            show this help message and exit
      -3, --three           Display three months spanning the date.
      -w, --weeknumbers     Display week numbers in the calendar
      -s, --sunday          Display Sunday as the first day of the week
      -f FILE, --file FILE  File with dates to highlight
      -d FORMAT, --format FORMAT
