---
title: "Verify Your SQL Connection / Get Connection String"
date: 2012-05-30T02:33:00+0000
categories: ["SQL"]
tags: ["Connections", "Troubleshooting"]
legacy: true
---

> **Note:** This post references older technology versions and may contain outdated information.

I have been very fortunate to have some fantastic mentors.  One of them is a gentleman by the name of Ken Ammann, who taught me this trick when you need to verify your SQL connection or get your connection string.

1) From your desktop, right-click and create a new text document.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiBCxVOs3a89eltIaeOUEkf3nNN4_gc83a8NpbyNsllzGMIs-0ClQa34KAFG5srf7rAhyphenhyphent7GvA7rCuFJgDZghuRxTdn-kVfuvr3HH9xvOAWif-ysR3x7Sb0lZR6G2gcc07AxuNmRokQosNM/s320/Text+Document.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiBCxVOs3a89eltIaeOUEkf3nNN4_gc83a8NpbyNsllzGMIs-0ClQa34KAFG5srf7rAhyphenhyphent7GvA7rCuFJgDZghuRxTdn-kVfuvr3HH9xvOAWif-ysR3x7Sb0lZR6G2gcc07AxuNmRokQosNM/s1600/Text+Document.jpg)

2) Change the file extension from .txt to .udl

[
](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKAA0kkBuv7aSWvDjxyyKkBofwro8rIjyCO-IbBVxHUp7g-hnI_dhakrHGNI7w8urHiy1deNseIW1Dkg1ye_r4RD1X-gdJreGRk5QBNz2Ht_ACuXk9zm8xr-E37kSFb6smQfsX1qa91_yo/s1600/udl+doc.jpg)[
](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKAA0kkBuv7aSWvDjxyyKkBofwro8rIjyCO-IbBVxHUp7g-hnI_dhakrHGNI7w8urHiy1deNseIW1Dkg1ye_r4RD1X-gdJreGRk5QBNz2Ht_ACuXk9zm8xr-E37kSFb6smQfsX1qa91_yo/s1600/udl+doc.jpg)[
](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKAA0kkBuv7aSWvDjxyyKkBofwro8rIjyCO-IbBVxHUp7g-hnI_dhakrHGNI7w8urHiy1deNseIW1Dkg1ye_r4RD1X-gdJreGRk5QBNz2Ht_ACuXk9zm8xr-E37kSFb6smQfsX1qa91_yo/s1600/udl+doc.jpg)[
](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKAA0kkBuv7aSWvDjxyyKkBofwro8rIjyCO-IbBVxHUp7g-hnI_dhakrHGNI7w8urHiy1deNseIW1Dkg1ye_r4RD1X-gdJreGRk5QBNz2Ht_ACuXk9zm8xr-E37kSFb6smQfsX1qa91_yo/s1600/udl+doc.jpg)[
](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKAA0kkBuv7aSWvDjxyyKkBofwro8rIjyCO-IbBVxHUp7g-hnI_dhakrHGNI7w8urHiy1deNseIW1Dkg1ye_r4RD1X-gdJreGRk5QBNz2Ht_ACuXk9zm8xr-E37kSFb6smQfsX1qa91_yo/s1600/udl+doc.jpg)[
](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKAA0kkBuv7aSWvDjxyyKkBofwro8rIjyCO-IbBVxHUp7g-hnI_dhakrHGNI7w8urHiy1deNseIW1Dkg1ye_r4RD1X-gdJreGRk5QBNz2Ht_ACuXk9zm8xr-E37kSFb6smQfsX1qa91_yo/s1600/udl+doc.jpg)[
](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKAA0kkBuv7aSWvDjxyyKkBofwro8rIjyCO-IbBVxHUp7g-hnI_dhakrHGNI7w8urHiy1deNseIW1Dkg1ye_r4RD1X-gdJreGRk5QBNz2Ht_ACuXk9zm8xr-E37kSFb6smQfsX1qa91_yo/s1600/udl+doc.jpg)[
](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKAA0kkBuv7aSWvDjxyyKkBofwro8rIjyCO-IbBVxHUp7g-hnI_dhakrHGNI7w8urHiy1deNseIW1Dkg1ye_r4RD1X-gdJreGRk5QBNz2Ht_ACuXk9zm8xr-E37kSFb6smQfsX1qa91_yo/s1600/udl+doc.jpg)

[](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhsilwzOj4N8nE72oGFFiKC7hlQKzsGGIdCRyBhWymS6doqVqMNw7GbS3_rxb65mzS0PVOLkJPBHTAHbANzkEQSwAB7qd028WAp5QsmSre8i_HXEeIEujLmDmAfsk9VMvfNwA1X0-XpWPiL/s1600/change+to+udl.jpg)[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhsilwzOj4N8nE72oGFFiKC7hlQKzsGGIdCRyBhWymS6doqVqMNw7GbS3_rxb65mzS0PVOLkJPBHTAHbANzkEQSwAB7qd028WAp5QsmSre8i_HXEeIEujLmDmAfsk9VMvfNwA1X0-XpWPiL/s1600/change+to+udl.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhsilwzOj4N8nE72oGFFiKC7hlQKzsGGIdCRyBhWymS6doqVqMNw7GbS3_rxb65mzS0PVOLkJPBHTAHbANzkEQSwAB7qd028WAp5QsmSre8i_HXEeIEujLmDmAfsk9VMvfNwA1X0-XpWPiL/s1600/change+to+udl.jpg)

becomes:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjZ6Fx-gF8cr4oQ2AexTk7QdZZKLnCFk0TqiCQM6DPBpyeicqLfFEM7DmsJnrtIzlivAQSw50kGzMomTa0wKAK4Z_rNnCQr6PVZ24iOGMO_duX2rOrD0LA5eGQ9kRgrJ7gNrUlWWqRDN_fX/s1600/udl+doc.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjZ6Fx-gF8cr4oQ2AexTk7QdZZKLnCFk0TqiCQM6DPBpyeicqLfFEM7DmsJnrtIzlivAQSw50kGzMomTa0wKAK4Z_rNnCQr6PVZ24iOGMO_duX2rOrD0LA5eGQ9kRgrJ7gNrUlWWqRDN_fX/s1600/udl+doc.jpg)

3) Click Yes to verify the change of extension type.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgEIJZqyTx-dEhkV7fOm5MQkCNNEF5cYKwuXoDeh4_RdbcCAT0cYoxQ84dS2R7IOB-tfR82LZfmMZTcY47mgMBLE-8AsQHA-hMrUA0BYLQhtZQnMddjaOsFjYkdPamOgdqvev-iabe4xf2-/s320/yes+to+change.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgEIJZqyTx-dEhkV7fOm5MQkCNNEF5cYKwuXoDeh4_RdbcCAT0cYoxQ84dS2R7IOB-tfR82LZfmMZTcY47mgMBLE-8AsQHA-hMrUA0BYLQhtZQnMddjaOsFjYkdPamOgdqvev-iabe4xf2-/s1600/yes+to+change.jpg)

4) Open the UDL file and set properties.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEv1P9dWRMXcJ-xoBIUxYsC_xhdBZmMMJT68pGnAIUaLEETOjO2D7C2c6pcxagAqLGyprqbEFOuhwGJ_D6wBl7i9PW43uHM-aCH-fGvEqe0pFHnz1pQM8SZHmR4f2hjB-CpzNmqsAku36U/s320/set+properties.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEv1P9dWRMXcJ-xoBIUxYsC_xhdBZmMMJT68pGnAIUaLEETOjO2D7C2c6pcxagAqLGyprqbEFOuhwGJ_D6wBl7i9PW43uHM-aCH-fGvEqe0pFHnz1pQM8SZHmR4f2hjB-CpzNmqsAku36U/s1600/set+properties.jpg)

5) Test the connection.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhRYAwyulPywVHGuXk8SCh4StTNFk5rVyVgYvcPpwQpqhICN9uReL0kSqxbanpffhqnOHFQTUS9JMFeoNT_TX5L32Lwy01APEZT9W2LeOqBzwcob3PvLQssA2-7nVKvjCK9xMjBLVUJ1Rkm/s1600/verify+connection.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhRYAwyulPywVHGuXk8SCh4StTNFk5rVyVgYvcPpwQpqhICN9uReL0kSqxbanpffhqnOHFQTUS9JMFeoNT_TX5L32Lwy01APEZT9W2LeOqBzwcob3PvLQssA2-7nVKvjCK9xMjBLVUJ1Rkm/s1600/verify+connection.jpg)

6) Click OK.

7) Change file extension back to .txt

8) Take a look at your connection string.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgtY5-TKg6A9UlPYWuJsf5LNH2anJv738_WJXHWmiwUFT211-hUmDryb-Oipy09hMXxwq43ApG0D-m9gu4_Zz9moqldhgYtMcYhXgiy-QK1W5mJIeUkG75zMqzVAeUvx0bBAjxOCLpN938i/s640/connection+string.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgtY5-TKg6A9UlPYWuJsf5LNH2anJv738_WJXHWmiwUFT211-hUmDryb-Oipy09hMXxwq43ApG0D-m9gu4_Zz9moqldhgYtMcYhXgiy-QK1W5mJIeUkG75zMqzVAeUvx0bBAjxOCLpN938i/s1600/connection+string.jpg)

It is a nice easy way to verify your SQL connections when you need to verify that you still have a SQL connection.  The real reason that I wrote this post is that I was spending too much time trying to recall the UDL extension name.
