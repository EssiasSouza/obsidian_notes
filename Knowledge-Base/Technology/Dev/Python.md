Source: #source/internet_resources 
Project: #project/python
Areas: #area/work
Subject: #subect/python
Type: #type/learning 
Learning priority: #priority/P2 
Status: #status/learning
Related: 

---
# logging

```
import logging

logging.basicConfig(
    filename="app.log",
    level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s"
)

logging.info("Application started")
```