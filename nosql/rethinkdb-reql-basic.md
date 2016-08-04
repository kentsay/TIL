# RethinkDB and ReQL tutorial
This note includes some introduction and usage about RethinkDB and it's own query language - ReQL. RethinkDB is a open-source, scalable JSON database for the realtime web that I recently start to use for my online service. ReQL is the RethinkDB query language. It embeds into your programming language, are all chainable (a bit like scala), and all queries execute on server.
If you want to know more about RethinkDB, you can read the [faq](https://www.rethinkdb.com/faq/) on it's official website, or read this article: [MongoDB v.s. RethinkDB](https://www.amon.cx/blog/rethinkdb-reviewed-by-a-mongo-fan/)

### Basic command
* Start the server from a terminal window:
```
rethinkdb
```
RethinkDB provides you a admin web ui to monitor and manipulate your server: `http://localhost:8080`. If you are not able to see this, try this command:
```
rethinkdb --bind all
```

* ReQL with python
  * import the driver
```python
import rethinkdb as r
```
  * open a connection
```python
r.connect( "localhost", 28015).repl()
```
  * create a new table
```python
r.db("test").table_create("authors").run()
```
  * insert data
  ```python
  r.table("authors").insert([
    { "name": "William Adama", "tv_show": "Battlestar Galactica",
      "posts": [
        {"title": "Decommissioning speech", "content": "The Cylon War is long over..."},
        {"title": "We are at war", "content": "Moments ago, this ship received..."},
        {"title": "The new Earth", "content": "The discoveries of the past few days..."}
      ]
    }
    ]).run()
  ```
  * retrieve documents
```python
cursor = r.table("authors").run()
for document in cursor:
  print(document)
```
  * realtime feeds
```python
cursor = r.table("authors").changes().run()
for document in cursor:
    print(document)
```
  * update all documents
```python
r.table("authors").update({"type": "fictional"}).run()
```
  * update specific document
```python
r.table("authors").
    filter(r.row['name'] == "William Adama").
    update({"rank": "Admiral"}).run()
```
  * delete documents
```python
r.table("authors").
    filter( r.row["posts"].count() < 3 ).
    delete().run()
```

### Reference
* [RethinkDB Architecture](https://www.rethinkdb.com/docs/architecture/)
* [RethinkDB 10 mins guide](https://www.rethinkdb.com/docs/guide/python/)
* [Introduction to ReQL](https://www.rethinkdb.com/docs/introduction-to-reql/)
