#Android ANR

### What does ANR means
In Android, the system guards against applications that are insufficiently responsive for a period of time by displaying a dialog that says your app has stopped responding, which is an "Application Not Responding" (ANR) dialog. At this point, your app has been unresponsive for a considerable period of time so the system offers the user an option to quit the app. It's critical to design responsiveness into your application so the system never displays an ANR dialog to the user.

### When does ANR occurs
Generally, the system displays an ANR if an application cannot respond to user input. For example, if an application blocks on some I/O operation (frequently a network access) on the UI thread so the system can't process incoming user input events. Or perhaps the app spends too much time building an elaborate in-memory structure or computing the next move in a game on the UI thread. It's always important to make sure these computations are efficient, but even the most efficient code still takes time to run.

### How to Avoid ANRs
Android applications normally run entirely on a single thread by default the "UI thread" or "main thread"). This means anything your application is doing in the UI thread that takes a long time to complete can trigger the ANR dialog because your application is not giving itself a chance to handle the input event or intent broadcasts.

Therefore, any method that runs in the UI thread should do as little work as possible on that thread. In particular, activities should do as little as possible to set up in key life-cycle methods such as `onCreate()` and `onResume()`. Potentially long running operations such as network or database operations, or computationally expensive calculations such as resizing bitmaps should be done in a worker thread (or in the case of databases operations, via an asynchronous request).

The most effective way to create a worker thread for longer operations is with the `AsyncTask` class. Simply extend AsyncTask and implement the `doInBackground()` method to perform the work. To post progress changes to the user, you can call publishProgress(), which invokes the onProgressUpdate() callback method. From your implementation of onProgressUpdate() (which runs on the UI thread), you can notify the user. For example:

```java
private class DownloadFilesTask extends AsyncTask<URL, Integer, Long> {
    // Do the long-running work in here
    protected Long doInBackground(URL... urls) {
        int count = urls.length;
        long totalSize = 0;
        for (int i = 0; i < count; i++) {
            totalSize += Downloader.downloadFile(urls[i]);
            publishProgress((int) ((i / (float) count) * 100));
            // Escape early if cancel() is called
            if (isCancelled()) break;
        }
        return totalSize;
    }

    // This is called each time you call publishProgress()
    protected void onProgressUpdate(Integer... progress) {
        setProgressPercent(progress[0]);
    }

    // This is called when doInBackground() is finished
    protected void onPostExecute(Long result) {
        showNotification("Downloaded " + result + " bytes");
    }
}
```
### Reference
* [Keeping Your App Responsive](http://developer.android.com/intl/in/training/articles/perf-anr.html)

