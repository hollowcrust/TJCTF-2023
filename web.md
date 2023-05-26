# TJCTF 2023 - Web Challenges

## #1: hi

### Description/Sources<br/>

<img width="698" alt="Screenshot 2023-05-27 at 00 02 37" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/6cd27a0a-eeb4-4ab5-8e7a-43741bc5dca6"><br/>

<a href="https://hi.tjc.tf">hi</a><br/>

The link directs us to the following website<br/><br>
<img width="1440" alt="Screenshot 2023-05-27 at 00 12 04" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/016a7f0a-a712-4733-b222-e04c1914f8c3"><br/>

Inspecting the website, we can see that the flag is hidden under a canvas<br/><br/>
<img width="450" alt="Screenshot 2023-05-27 at 00 15 06" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/18d8e502-2c21-4cd4-b51b-98dbb5ca2c66"><br/>

We can remove the line above the flag image to reveal the flag<br/><br/>
<img width="1440" alt="Screenshot 2023-05-27 at 00 17 11" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/5e36640c-04e2-41f0-b8cb-84f6dfcf6e6d"><br/>

### Flag
```
tjctf{pretty_canvas_577f7045}
```

## #2: swill-squill

### Description/Sources<br/>
<img width="699" alt="Screenshot 2023-05-27 at 00 18 59" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/28be45e4-66e3-486e-8f37-486c724d7730"><br/>

<a href="https://instancer.tjctf.org/challenge/swill-squill">Instancer</a><br/>

The Instancer leads us to the following website <br/><br/>
<img width="1440" alt="Screenshot 2023-05-27 at 00 30 44" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/841e6d89-7635-4ecf-bb83-af823c5aabba"><br/>

From `app.py` in the `server` folder
```Python
c = conn.cursor()

string = "SELECT description FROM notes WHERE owner == '" + name + "';"
c.execute(string)
```

We can perform an SQL injection whereby if we put `' OR 1=1; --` in the `Name` field, the SQL command above will become `"SELECT description FROM notes WHERE owner == '' OR 1=1; --';"`. The `--` turns everything behind it on that line into a comment, and since `'' OR 1=1` always return `True`, the website will display everything from every `owner` in the database.</br>

<img width="635" alt="Screenshot 2023-05-27 at 00 31 44" src="https://github.com/hollowcrust/TJCTF-2023/assets/72879387/38040aa9-7b32-4c85-9990-4c8548a1c184"><br/>

### Flag
```
tjctf{swill_sql_1y1029345029374}
```

