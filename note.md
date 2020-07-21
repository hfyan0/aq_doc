# Server restart procedures

1. Kill running instances
  - omdc
  - aqbss
  - data order server

  ```
  pgrep omdc
  pgrep aqbss
  pgrep dataorderserver
  ```

  ```
  kill $(pgrep omdc)
  kill $(pgrep aqbss)
  kill $(pgrep dataorderserver)
  ```

2. Restart OMDC
  - Add one-off job in crontab
    ```
    crontab -e
    ```
  - Start omdc SM first
  - Check the log and wait for "Completed Refreshing"
  - Check omdc log file for "Not Match"

    ```
    grep "Not Match" D1.txt
    ```
  - If "Not Match" is present, kill OMDC and restart again.
  - Use tradingsim.x to check the correctness of data
    ```
    tradingsim.x 10 100
    tradingsim.x 10 100 | grep "000 700 "
    tradingsim.x 10 100 | grep "000 388 "
    ```

  - Start omdc UDP
  - Follow the same procedures as OMDC SM, except using udpreceiver to check the correctness of data
    ```
    ./udpreceiver 192.168.150.104 238.0.9.11 29000
    ./udpreceiver 192.168.150.104 238.0.9.11 29000 | grep "000 700 "
    ./udpreceiver 192.168.150.104 238.0.9.11 29000 | grep "000 388 "
    ```


3. Restart AQBSS
  ```
  crontab -e
  ```

4. Restart Data Order Server

  ```
  crontab -e
  ```
  - make sure the first data order server is started at least 2 minutes after AQBSS.
  - start every 2 data order server every minute.

5. Check logs. Same procedures as in the morning routine check.

6. **[Important!!]** Remember to remove all the one-off entries from crontab
