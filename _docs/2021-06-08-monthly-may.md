---
title: "2021.05."
docs:
  - Docs
last_modified_at: 2021-06-08
author_profile: true
---

May 2021 Review.<br/>
With [YEBIT (예빛)](https://soundcloud.com/yebit)

## Applied CDL Quantum

I applied Creative Destruction Lab Quantum.
In this application, I must submit an assignment which is solving a max-cut problem using Quantum Approximate Optimization Algorithm (QAOA).
I do not know QAOA well so I refer to this video: [A tutorial on Quantum Approximate Optimization Algorithm](https://youtu.be/E0Sos_lR-kI).
The codes below are the last part of my answer.

```python
best_cut, best_solution = min([(maxcut(x), x) for x in counts.keys()], key=itemgetter(0))
print(f"Best string: {best_solution} with cut: {-best_cut}")

# Sort nodes in two lines: 0 (red) or 1 (blue)
colors = ['r' if best_solution[node] == '0' else 'b' for node in G]
lst = []
for node in G:
  if (best_solution[node] == '0'): lst.append(node)
nx.draw(G, node_color=colors, pos=nx.bipartite_layout(G, lst))
```

Sadly, there is no reply from CDL but I think the try is good.
If I did not apply it, I still do not know QAOA at all. (lol)
I should look forward to the next chance!

## Java assignments

These days, I study Java at my college.
The reason why I talk about this Java class is assignments are really interesting.
Every class has an assignment and I really liked it combined with GUI (Swing).
I uploaded one of my assignments in my til: [TextShift.java](https://github.com/tula3and/til/blob/master/Java/Examples/TextShift.java).
In this Java file, texts are moving when I pressed `→` or `←`.
I should give a focus to the label for moving texts right after executing, so I used the codes below.

```java
label.grabFocus();
label.requestFocus();
```

FYI: Its length is very short but I spent quite a long time to search this.

## I got an iPad Pro!

I bought an iPad Pro (4th generation).
I restarted drawing and recently uploaded a picture in Behance.
Actually this is my first time to buy a material from Apple and I am satisfied than I expected!

<img src="https://user-images.githubusercontent.com/62553200/121156342-8f935300-c883-11eb-95d9-870a36d2432c.jpg" width="700">

I am truly considering buying a MacBook Pro...
