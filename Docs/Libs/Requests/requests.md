# HTTP Requests in Python with `requests`

This document explains how to use the **`requests`** library to send HTTP requests, handle responses, work with APIs, upload/download files, manage headers, authentication, errors, and more.

> `requests` is not part of the standard library and must be installed:
```bash
pip install requests
```

---

## 1. Sending a Basic GET Request

### Code

```python
import requests

r = requests.get("http://myip.abbascloud.ir")

print(r.url)
print(r.status_code, r.ok)
print(r.text)
print(r.content)
print(r.json())
print(r.headers)
```

### Explanation

- `requests.get(url)` sends an HTTP GET request.
- `r.url`: final URL after redirects.
- `r.status_code`: HTTP status code (e.g. 200, 404).
- `r.ok`: `True` if status code is < 400.
- `r.text`: response body as string.
- `r.content`: response body as raw bytes.
- `r.json()`: parses JSON response into Python objects.
- `r.headers`: response headers as a dictionary.

---

## 2. Passing Query Parameters

### Code

```python
params = {
    "q": "python",
    "page": 1
}

response = requests.get("https://api.example.com/search", params=params)
print(response.url)
```

### Explanation

- `params` are appended to the URL as query strings.
- Automatically encoded by `requests`.
- Resulting URL:
  ```
  https://api.example.com/search?q=python&page=1
  ```

---

## 3. Sending POST Requests (Form Data)

### Code

```python
data = {
    "username": "alex",
    "password": "secret"
}

requests.post(url, data=data)
```

### Explanation

- Sends data as `application/x-www-form-urlencoded`.
- Common for HTML form submissions.

---

## 4. Sending POST Requests (JSON)

### Code

```python
json_data = {
    "name": "Alex",
    "age": 25
}

requests.post(url, json=json_data)
```

### Explanation

- Automatically serializes data to JSON.
- Sets `Content-Type: application/json`.
- Preferred for REST APIs.

---

## 5. Custom Request Headers

### Code

```python
headers = {
    "Authorization": "Bearer YOUR_TOKEN",
    "User-Agent": "MyApp/1.0",
    "Accept": "application/json"
}

response = requests.get(url, headers=headers)
```

### Explanation

- Used for authentication, API versioning, and content negotiation.
- Common headers:
  - `Authorization`
  - `User-Agent`
  - `Accept`

---

## 6. Authentication (Basic Auth)

### Code

```python
from requests.auth import HTTPBasicAuth

requests.get(
    "https://api.example.com",
    auth=HTTPBasicAuth("username", "password")
)
```

### Explanation

- Sends credentials using HTTP Basic Authentication.
- Automatically encodes credentials in headers.

---

## 7. Working with Cookies

### Code

```python
response = requests.get(url)
print(response.cookies)
```

### Explanation

- Cookies are stored in a `RequestsCookieJar`.
- Useful for sessions and login persistence.

---

## 8. Uploading Files

### Code

```python
files = {
    "file": open("example.txt", "rb")
}

response = requests.post(url, files=files)
```

### Explanation

- Sends files as `multipart/form-data`.
- Common for file uploads to APIs.

---

## 9. Downloading Files

### Simple Download

```python
response = requests.get("https://example.com/image.png")

with open("image.png", "wb") as f:
    f.write(response.content)
```

- Downloads entire file into memory.
- Suitable for small files.

---

### Streaming Large Files

```python
response = requests.get(url, stream=True)

with open("big.zip", "wb") as f:
    for chunk in response.iter_content(chunk_size=8192):
        f.write(chunk)
```

- Downloads file in chunks.
- Prevents high memory usage.
- Recommended for large files.

---

## 10. Timeout and Error Handling

### Code

```python
try:
    response = requests.get(url, timeout=5)
    response.raise_for_status()
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.HTTPError as e:
    print("HTTP error:", e)
except requests.exceptions.RequestException as e:
    print("Request failed:", e)
```

### Explanation

- `timeout`: maximum wait time (seconds).
- `raise_for_status()`: raises exception for 4xx / 5xx errors.
- `RequestException`: base class for all request errors.

---

## 11. Using Proxies

### Code

```python
proxies = {
    "http": "http://127.0.0.1:8080",
    "https": "http://127.0.0.1:8080"
}

requests.get(url, proxies=proxies)
```

### Explanation

- Routes requests through a proxy server.
- Useful for debugging, privacy, or corporate networks.

---

## 12. SSL Verification

### Code

```python
requests.get(url, verify=True)   # default
requests.get(url, verify=False)  # NOT recommended
```

### Explanation

- `verify=True` checks SSL certificates.
- Disabling SSL verification is insecure and should only be used for testing.

---

## 13. Real API Example (GitHub)

### Code

```python
import requests

API_URL = "https://api.github.com/users/octocat"

response = requests.get(API_URL)

if response.ok:
    user = response.json()
    print(user["login"], user["public_repos"])
else:
    print("Error:", response.status_code)
```

### Explanation

- Sends a GET request to GitHub’s public API.
- Parses JSON response.
- Accesses specific fields safely.
- Checks request success using `response.ok`.

---

## Summary

- `requests` simplifies HTTP communication
- Supports GET, POST, headers, params, JSON, files
- Handles authentication, cookies, proxies, and SSL
- Built-in error handling improves reliability
- Widely used for REST APIs and web services
