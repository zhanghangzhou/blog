Join
---
- Join(Inner Join)
> 内连接：产生两表的数据交集  
SELECT * FROM A INNER JOIN B ON A.username = B.username;

| username  | name  | age | username  | password  | valid |
| :-------  | :---- | :-- | :-------  | :-------  | :---- |
| user1     | John  | 20  | user1     | 1qazzaq1  | false |
| user3     | Tom   | 40  | user3     | 12345678  | true  |
| user5     | Alex  | 60  | user5     | 11111111  | true  |

- Left [Outer] Join
> 左外连接：产生左的完全集及右的对应集或null
SELECT * FROM A LEFT JOIN B ON A.username = B.username;

| username  | name  | age | username  | password  | valid |
| :-------  | :---- | :-- | :-------  | :-------  | :---- |
| user1     | John  | 20  | user1     | 1qazzaq1  | false |
| user2     | Mary  | 30  | null      | null      | null  |
| user3     | Tom   | 40  | user3     | 12345678  | true  |
| user4     | Jerry | 50  | null      | null      | null  |
| user5     | Alex  | 60  | user5     | 11111111  | true  |

- Right [Outer] Join
> 右外连接：产生右的完全集及右的对应集或null
SELECT * FROM A RIGHT JOIN B ON A.username = B.username;

| username  | name  | age | username  | password  | valid |
| :-------  | :---- | :-- | :-------  | :-------  | :---- |
| user1     | John  | 20  | user1     | 1qazzaq1  | false |
| user3     | Tom   | 40  | user3     | 12345678  | true  |
| user5     | Alex  | 60  | user5     | 11111111  | true  |
| null      | null  | null| user7     | 88888888  | false |
| null      | null  | null| user9     | 12121212  | true  |

- Full [Outer] Join
> 全外连接：产生左右的并集，无对应数据为null
SELECT * FROM A FULL JOIN B ON A.username = B.username;

| username  | name  | age | username  | password  | valid |
| :-------  | :---- | :-- | :-------  | :-------  | :---- |
| user1     | John  | 20  | user1     | 1qazzaq1  | false |
| user2     | Mary  | 30  | null      | null      | null  |
| user3     | Tom   | 40  | user3     | 12345678  | true  |
| user4     | Jerry | 50  | null      | null      | null  |
| user5     | Alex  | 60  | user5     | 11111111  | true  |
| null      | null  | null| user7     | 88888888  | false |
| null      | null  | null| user9     | 12121212  | true  |

- Cross Join
> 交叉连接：两个表的笛卡尔积  
SELECT * FROM A CROSS JOIN B ON A.username = B.username;

| username  | name  | age | username  | password  | valid |
| :-------  | :---- | :-- | :-------  | :-------  | :---- |
| user1     | John  | 20  | user1     | 1qazzaq1  | false |
| user1     | John  | 20  | user3     | 12345678  | true  |
| ...       | ...   | ... | ...       | ...       | ...   |
> 隐藏更多的交叉连接的查询结果集。

- 附录：测试表

Table A:

| username  | name    | age   |
| :-------  | :---    | :---: |
| user1     | John    | 20    |
| user2     | Mary    | 30    |
| user3     | Tom     | 40    |
| user4     | Jerry   | 50    |
| user5     | Alex    | 60    |

Table B:

| username | password | valid |
| :------- | :------- | :---  |
| user1    | 1qazzaq1 | false |
| user3    | 12345678 | true  |
| user5    | 11111111 | true  |
| user7    | 88888888 | false |
| user9    | 12121212 | true  |
