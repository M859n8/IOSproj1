# First project for Operating Systems (IOS). 

## Description

The task is to develop a shell script that processes logs from a cryptocurrency exchange. The script should filter and sort transaction records for individual users based on various filters like date, time, and currency. It will also support commands to display user account information, sorted lists of currencies, account statuses, and fictional profits. The focus of the project is on implementing efficient log processing, filtering, and formatting of the data in a user-friendly way.

## Script Interface Specification
xtf - a script for preprocessing logs from your cryptocurrency exchange.

``` bash
xtf [-h|--help] [FILTER] [COMMAND] USER LOG [LOG2 [...] 
```
### COMMAND options:
 - list – list records for a given user.
 - list-currency – output a sorted list of currencies that appear.
 - status – display the actual account status, grouped and sorted by currency.
 - profit – display the customer’s account status with fictional returns included.
### FILTER options (can be combined):
 - -a DATETIME – after: only records after this date and time are considered. DATETIME format is YYYY-MM-DD HH:MM:SS.
 - -b DATETIME – before: only records before this date and time are considered.
 - -c CURRENCY – only records for the specified currency are considered.
### Help option:
 - -h or --help will display a help message with a brief description of each command and switch.

 ### Examples
 ```bash
$ cat cryptoexchange.log
Trader1;2024-01-15 15:30:42;EUR;-2000.0000
Trader2;2024-01-15 15:31:12;BTC;-9.8734
Trader1;2024-01-16 18:06:32;USD;-3000.0000
CryptoWiz;2024-01-17 08:58:09;CZK;10000.0000
Trader1;2024-01-20 11:43:02;ETH;1.9417
Trader1;2024-01-22 09:17:40;ETH;10.9537
$ ./xtf Trader1 cryptoexchange.log
Trader1;2024-01-15 15:30:42;EUR;-2000.0000
Trader1;2024-01-16 18:06:32;USD;-3000.0000
Trader1;2024-01-20 11:43:02;ETH;1.9417
Trader1;2024-01-22 09:17:40;ETH;10.9537
$ ./xtf list Trader1 cryptoexchange.log
Trader1;2024-01-15 15:30:42;EUR;-2000.0000
Trader1;2024-01-16 18:06:32;USD;-3000.0000
Trader1;2024-01-20 11:43:02;ETH;1.9417
Trader1;2024-01-22 09:17:40;ETH;10.9537
$ ./xtf -c ETH Trader1 cryptoexchange.log
Trader1;2024-01-20 11:43:02;ETH;1.9417
Trader1;2024-01-22 09:17:40;ETH;10.9537
$ ./xtf -c GBP Trader1 cryptoexchange.log
$
$ ./xtf list-currency Trader1 cryptoexchange.log
ETH
EUR
USD
$ ./xtf status Trader1 cryptoexchange.log
ETH : 12.8954
EUR : -2000.0000
USD : -3000.0000
$ ./xtf -b "2024-01-22 09:17:40" status Trader1 cryptoexchange.log
ETH : 1.9417
EUR : -2000.0000
USD : -3000.0000
$ ./xtf -a "2024-01-15 16:00:00" -b "2024-01-22 09:17:41" status Trader1 cryptoexchange.log
ETH : 12.8954
USD : -3000.0000
$ ./xtf profit Trader1 cryptoexchange.log
ETH : 15.4744
EUR : -2000.0000
USD : -3000.0000
export XTF_PROFIT=40
$ ./xtf profit Trader1 cryptoexchange.log
ETH : 18.0535
EUR : -2000.0000
USD : -3000.0000
Příklad s více logy:

$ cat cryptoexchange-1.log
Trader1;2024-01-15 15:30:42;EUR;-2000.0000
Trader2;2024-01-15 15:31:12;BTC;-9.8734
Trader1;2024-01-16 18:06:32;USD;-3000.0000
$
$ gunzip -ck cryptoexchange-2.log.gz
CryptoWiz;2024-01-17 08:58:09;CZK;10000.0000
Trader1;2024-01-20 11:43:02;ETH;1.9417
Trader1;2024-01-22 09:17:40;ETH;10.9537
$
$ ./xtf status Trader1 cryptoexchange-1.log cryptoexchange-2.log.gz
ETH : 12.8954
EUR : -2000.0000
USD : -3000.0000
Příklady rozšíření:

$ ./xtf Trader1 status -a "2024-01-15 16:00:00" -b "2024-01-22 09:17:41" cryptoexchange.log
ETH : 12.8954
USD : -3000.0000
$ ./xtf -c ETH -c USD Trader1 cryptoexchange.log
Trader1;2024-01-16 18:06:32;USD;-3000.0000
Trader1;2024-01-20 11:43:02;ETH;1.9417
Trader1;2024-01-22 09:17:40;ETH;10.9537
$ ./xtf -c ETH -c EUR -c GBP list-currency Trader1 cryptoexchange.log
ETH
EUR
```

