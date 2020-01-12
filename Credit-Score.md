Used to check the funneling of money from one account to another.

Inferences based on Dalal Street 2019

## # Gives Number of times user sold the stock to the same user:

```
SELECT u.email, COUNT(u.email) FROM `OrderFills` t, `Bids` b, `Users` u, `Asks` a WHERE (t.bidId = b.id AND a.userId = 359 AND a.id = t.askId AND b.userID = u.id) GROUP BY u.email ORDER BY COUNT(u.email) DESC

```
```
+-----------------------------------------+----------------+
| email                                   | COUNT(u.email) |
+-----------------------------------------+----------------+
| bestfgl12@gmail.com                     |             19 |
| sumanthchevula@gmail.com                |             19 |
| tejasf1234@mailinator.com               |             16 |
| tejasdummyf12@gmail.com                 |             12 |
| a@mailinator.com                        |             11 |
| 107116035@nitt.edu                      |             10 |
| anuj@mailinator.com                     |             10 |
| namankarn99@gmail.com                   |              9 |
| satyam@mailinator.com                   |              8 |
| harshmahajan927@gmail.com               |              8 |
| amit@mailinator.com                     |              8 |
| mayank@mailinator.com                   |              8 |
| naman@mailinator.com                    |              7 |
| tejasf123@gmail.com                     |              6 |
```


Similarly Number of times he purchased from same mail.
```
+--------------------------------+----------------+
| email                          | COUNT(u.email) |
+--------------------------------+----------------+
| bestfgl12@gmail.com            |             19 |
| tejasdummyf12@gmail.com        |             18 |
| namankarn99@gmail.com          |             14 |
| tejasf1234@mailinator.com      |             12 |
| 107116035@nitt.edu             |             12 |
| rg.rohit1996@gmail.com         |             11 |
| a@mailinator.com               |             11 |
| anuj@mailinator.com            |              9 |
| amit@mailinator.com            |              8 |
```



