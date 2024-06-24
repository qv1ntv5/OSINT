### What is Crt.sh

Crt.sh is a website that allows to gather information from a domain through a certificate-based search. crt.sh provides a searchable database of certificate transparency logs. Certificate Transparency is an Internet security standard and open source framework for monitoring and auditing digital certificates.

We can use this website to get subdomains from a given domain, using the following script we can do a request and then parse the response to only get the subdomains.

```python
import sys
from bs4 import BeautifulSoup
import requests
import re

def main(domain):
    url = f'https://crt.sh/?q={domain}'
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    lista = soup.find_all(string=re.compile(domain))
    for item in range(2, len(lista)):
        print(lista[item])

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: python script.py <domain>")
        sys.exit(1)
    
    domain = sys.argv[1]
    main(domain)
```

The handicap using this tool is that we get a lot of subdomains that not necesarily are up. We have to filter the output with httpx in order to get the status code and remove from the list those that are denied or forbbiden.