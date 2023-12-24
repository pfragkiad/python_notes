# python_notes


## Multithreading

Simple Thread approach:

```python
import threading

def download_video(url):
    with yt_dlp.YoutubeDL() as ydl:
        ydl.download(url)

threads=[]
for url in videos:
    t = threading.Thread(target=download_video,args=(url,))
    t.start()
    threads.append(t)

for t in threads:
    t.join()
```


Threadpool control of number of threads:

```python
from  concurrent.futures import ThreadPoolExecutor 
import time

items = []

# Define a function that sleeps for a given number of seconds
def sleep_function(seconds):
    print(f"Sleeping for {seconds} seconds...")
    items.append(1)
    time.sleep(seconds)
    print(f"Finished sleeping for {seconds} seconds")

# Function to execute the sleep function using a thread pool
def execute_with_threadpool(num_threads):
    with ThreadPoolExecutor(max_workers=num_threads) as executor:
        # Submit tasks to the thread pool
        for i in range(num_threads):
            executor.submit(sleep_function, 5)

if __name__ == "__main__":
    max_threads = 50
    execute_with_threadpool(max_threads)
    execute_with_threadpool(max_threads)
    print(len(items))
```


Thread-safe list

```python
import threading

my_list = []
lock = threading.Lock()

def append_to_list(item):
    global my_list
    with lock:
        my_list.append(item)

# Create threads to append items to the list concurrently
threads = []
for i in range(5):
    thread = threading.Thread(target=append_to_list, args=(i,))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

print("Modified list:", my_list)

```
