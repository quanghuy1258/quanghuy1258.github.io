---
title: "First Post, Simple Beginning"
categories:
  - Mathematics
tags:
  - Algebra
mathjax: true
issue_id: 1
---

It is really difficult to choose a topic for the first post.
There are so many things I want to share.
However, I remember the jokes everybody here often talks each other about the beginning of something, like "Vạn sự khởi đầu nan, gian nan bắt đầu nản".
That sentence has a slight modification from the original one, but its meaning is the same as the quote, "The first step is always the hardest".
Therefore, the topic for the beginning of the blog or the year should be simple.

So, although not new, the first problem I want to introduce is as follows:

> Prove that there is no smallest positive real number $x = a + b\sqrt{2}$ where $a$ and $b$ are integers.

*Stronger form (See Update 18/08/2022): Prove that for any positive real number $\varepsilon$, there always exist two integers $a$ and $b$ such that $0 < a + b\sqrt{2} < \varepsilon$.*

Actually, I found out this problem after learning a theorem.
It is a nice theorem, but is not easy to prove, so I will present it in later posts.
At that time, using that theorem to solve was too simple, and I wanted to find another way using more primitive tools.
However, within about a year since it was stated out, I did still not have an answer.
Finally, I decided to find the solution on the Internet and it is such a beautiful solution that I still remember clearly.

<details>
<summary>Solution</summary>
We have $1 < \sqrt{2} < 2$, so $0 < -1 + \sqrt{2} < 1$. <br>
Then, we have:
$$\lim_{n \to + \infty} (-1 + \sqrt{2})^n = 0$$
But, $(-1 + \sqrt{2})^n$ always has the form of $a + b\sqrt{2}$ with $a, b \in \mathbb{Z}$. <br>
That means there is no smallest positive real number $x = a + b\sqrt{2}$ (problem solved).
</details>

**Update 18/08/2022:** Thanks to my student's finding, the result obtained is much stronger than the requirement of the original problem.
From the result, we can infer the stronger form which is mentioned above.
Meanwhile, from the original form, we only need to prove that for any pair of integer $(a, b)$ such that $a + b\sqrt{2} > 0$, there always exist two integers $a'$ and $b'$ satisfying $0 < a' + b'\sqrt{2} < a + b\sqrt{2}$.
Two statements are not equivalent.
The sequence $\left(1 + \frac{1}{n}\right)_{n=1}^\infty$ is an example to prove this.
Although there is no smallest positive real number in the sequence, all elements of the sequence are greater than $1$, which means that there exists one positive real number such that no element in the sequence is less than it.
What an interesting thought!
