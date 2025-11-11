```
ffuf -w yourwordlist -u https://<yoursite>/FUZZ
```

    -w is your wordlist or path to it
    -u is the URL of your site
    Keyword FUZZ defines, where the inputs from wordlist will be placed

Ffuf will print the results to console. If you want to save results to a file, add

    -o yourfilename.txt or
    -o yourfilename.json
    
Your fuzz tests can result to lots of HTTP status codes that you are not interested in. You can filter those out. You can also match the status codes that you want to see. For example, if you want to filter out HTTP status code 401, add

    -fc 401

If you want to match eg. HTTP status code 200, add

    -mc 200

To demonstrate this basic command, let's discover what kind of endpoints Presta API has. SecLists api-endpoints-res.txt wordlist was chosen for this test.

```
ffuf -w SecLists-master/Discovery/Web-Content/api/api-endpoints-res.txt -u https://yourkeyhere@yoururlhere/api/FUZZ -o results.json -mc 200
```
