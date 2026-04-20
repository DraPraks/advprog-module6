1. Single-threaded web server.
2. Milestone 2 reflection: `handle_connection` is doing two different jobs at once:
    -  reads enough of the incoming HTTP request to know when the browser has finished sending headers, then it builds a valid HTTP response for the browser to render.
    - `BufReader`, `.lines()`, `.take_while(|line| !line.is_empty())` --> stop at the blank line that ends the request headers. 
    - server reads the HTML file from disk, measures its length for the `Content-Length` header, and sends the headers and body together with `write_all`. ![Commit 2 screen capture](/images/commit2.png)

3. Commit 3 Reflection notes
    - Changed `handle_connection`; reads only the first request line, checks whether the browser asked for `GET / HTTP/1.1`. If the request is for `/`, the server sends a `200 OK` response & sends `hello.html`. else, the server switches to `404 NOT FOUND` and sends `404.html`.
    - Refactoring is needed because both branches follow the same pattern: choose a status line, choose a file, read the file, compute the content length, and build the final HTTP response. By storing the differences as `(status_line, filename)`, the repeated response-building code only appears once, which makes the function easier to read and easier to extend later.
    - Screenshot of my Milestone 3 error page with my own message: ![Commit 3 screen capture](/images/commit3.png)