# EXP 2 - Word Count using Hadoop Streaming

## 📁 Directory Setup

```bash
mkdir regno
cd regno
```

## 📄 Dataset.txt

Create a file named `Dataset.txt` and add the following content:

```text
A car is an essential part of our life. We use the car to travel from one place to another.
A car has an engine, four wheels, four doors, one boot, four windows, brakes, accelerators, headlights, etc.
The car runs on various fossil fuels like petrol, diesel or CNG. But, today, many car companies are launching cars that run on electricity.
There are a lot of car companies that design cars with ultra-luxurious interiors and exteriors.
Every year you get to hear about the launches of new cars, and each car is unique in its way.
The car comes in automatic and manual modes, making travel more comfortable and safe.
The invention of the car has made transport more accessible and more manageable.
It also saves time because it reduces travelling time when compared to public transport systems.
Travelling in a car is always fun and memorable.
It is always beneficial to have your own car, especially in emergencies.
We don’t have to depend on others to take us to our destination.
We can drive by ourselves and reach our destination.
If we have our own car, we can explore new places and go for a long drive whenever we feel like it.
Travelling in a car is always fun and memorable.
It gives us an incredible feeling as we enjoy the ride sitting in the comfy seats, turning on the AC and listening to our favourite songs.
We all dream of buying our own car, but owning one becomes expensive.
When we step out of our homes, we see many cars plying on the road.
But there are many who can’t afford to buy one.
```

Save with `CTRL+S`.

## 🧠 Mapper Script

Save this as `mapper.py`:

```python
#!/usr/bin/env python
import sys

for line in sys.stdin:
    line = line.strip()
    words = line.split()
    for word in words:
        print("%s\t1" % word)
```

## 🧠 Reducer Script

Save this as `reducer.py`:

```python
#!/usr/bin/env python
import sys

current_word = None
current_count = 0

for line in sys.stdin:
    line = line.strip()
    try:
        word, count = line.split('\t', 1)
        count = int(count)
    except ValueError:
        continue

    if current_word == word:
        current_count += count
    else:
        if current_word:
            print("%s\t%d" % (current_word, current_count))
        current_word = word
        current_count = count

if current_word == word:
    print("%s\t%d" % (current_word, current_count))
```

Save with `CTRL+S`.

## 🔧 Make scripts executable

```bash
chmod +x mapper.py reducer.py
```

## 📂 Upload Data to HDFS

```bash
hadoop fs -mkdir -p /input
hadoop fs -put Dataset.txt /input/
```

## 🚀 Run Hadoop Streaming Job

```bash
hadoop jar /usr/lib/hadoop-mapreduce/hadoop-streaming.jar --input /input --output /output --mapper mapper.py --reducer reducer.py --file mapper.py --file reducer.py
```

## 📤 View Output

```bash
hadoop fs -cat /output/part-00000
```
