# NodeJS Strange

## Bug

By changing `Content-Type` from `text/plain` to `test/plain` I accidentally forced the browser to **download** the page after loading it.

```js
const http = require("node:http");
const fs = require("node:fs/promises");

const PORT = 8000;

const server = http.createServer();

server.on("request", async (req, res) => {
  const contentBuffer = await fs.readFile(__dirname + "/test.txt");

  res.statusCode = 200;
  // HERE
  res.setHeader("Content-Type", "test/plain");
  res.end(contentBuffer.toString("utf-8"));
});

server.listen(PORT, () => {
  console.log(`Server running at http://localhost:8000`);
});
```
