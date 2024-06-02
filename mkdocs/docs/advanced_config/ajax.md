# Ajax request settings

- We send Ajax requests to `ajax/del_entries.php` with data as an array of ids. In order to stop the POST request getting too large and triggering a 406 error, or else exceeding the maximum size of an SQL query, we split the request into batches with the maximum number of ids in the array defined below.

    ```php-inline
    $del_entries_ajax_batch_size = 5000;
    ```

- The maximum number of parallel requests to `ajax/del_entries.php`. Increasing this number will increase the speed of processing, but if it's too large will increase the load on the server and possibly cause errors.

    ```php-inline
    $del_entries_parallel_requests = 2;
    ```

- Only required if your MRBS installation runs from a Git repository and you want the "Help" page to show the Git commit ID you are on. Default should work if "git" is in your search path, on Windows you may need to specify the full path to your "git" executable, e.g.: `c:/Program Files/TortoiseGit/git.exe`

    ```php-inline
    $git_command = "git";
    ```