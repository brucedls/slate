# REST API FAQs

1. API call limits

Answer: An IP can send up to 10 https requests per second. If the number of requests exceeded 10, the IP will be blocked for one hour and automatically unblocked after an hour. Some interfaces also have API call frequency restrictions for the same key. Specific restrictions can be found in the relevant interfaces. If the API call frequency is lowered later. the access will be restored.

2. The server returns error code 10000.

Answer: Please check whether the parameters required by the interface are submitted to the server correctly, and then check whether the content type of the request header information is misconfigured as `'application/x-www-form-urlencoded'`.