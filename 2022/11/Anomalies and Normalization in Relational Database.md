## The Anomalies

Before we move on to the Normal Forms we may first know about the anomalies.

- Update Anomalies
	- You may update data you do not want to update
- Delete Anomalies
	- You may delete data you do not want to delete
	- You may delete additional information
- Insert Anomalies
	- You may need extra information that is not necessarily required to insert a data


### Examples

**1). Update Anomalies**

![](https://i.imgur.com/hrtbzzZ.png)

For example, when we are trying to `update` the `Address` of Student `Hank`, we may finally update two rows. That's what we don't want.

**2). Delete Anomalies**

We still take the previous table as example.

First, if we want to delete `Hank`, we may finally delete two rows.

Besides, if we want to delete `King`, if he is the only student who take course `Data Science`, then we may lose this course in the table. That means we are deleting **additional information**.

**3). Insert Anomalies**

Also the same table. If we want to add a new student **who didn't take any courses**, how can we suppose to do that? Same to the courses, we cannot add a new course unless one student registered it, [Chicken or the egg?](https://en.wikipedia.org/wiki/Chicken_or_the_egg). 

That means, if we want to insert a new row, we have to know some additional information that is not necessary, which is not good.

## Normalization Forms (NF)

Normalization is to resolve the anomalies problems.

0: UNF - Unnormalized Form

1. 1NF
	- No repeating groups and with PK identified
2. 2NF
	- 1NF and no partial dependencies
3. 3NF
	- 2NF and no transitive dependencies
4. (BCNF)
5. (4NF)

Typically we do not go beyond the 3NF.

## Important Concepts

### About Keys

- Super Key: All keys and key combinations that can "uniquely identify" a row.
- Candidate Key: Minimum attribute in the super key.
- Primary Key: We can choose a candidate key to be a primary key, typically this is the most important candidate key.

### About Dependency

- Functional Dependence: I would say that is one A can "**uniquely identify**" one B.
- Partial Dependency: functional dependence in which the determinant is only part of the primary key
- Transitive Dependency: attribute is dependent on another attribute that is not part of the primary key

**Functional Dependency Examples**

I'm going to bring down the same table here:

![](https://i.imgur.com/hrtbzzZ.png)

When you look at `StudentNo`, with `StudentName` and `Address`, you can find that for each `StudentNo`, we will get the unique other attributes (although there are multiple rows). 

For example:

- S10 -> Hank, Toronto
- S11 -> Thomas, Markham
- S12 -> King, Calgary

That is the functional dependency, `StudentNo` can functionally determine `StudentName` and `Address`. Written as `StudentNo -> StudentName, Address`.

Can we write it in the reverse order? (That is `StudentName, Address -> StudentNo`)

For current table, probably yes. But when we add more data into the table, if there's two `Hank` both live in `Toronto`, and they are two students (different id), then we can't say there's a functional dependency for `StudentName, Address -> StudentNo`.

However, when it comes to `StudentNo` and `Course`, we can find that:

* S10
	* Java
	* Database
* S12
	* Web
	* Data Science

So `StudentNo` cannot functionally determine `Course`.

Same for the `CourseNo` to other attributes than `Course`.

## Normalization

### Moving to 1NF

![](https://i.imgur.com/hrtbzzZ.png)

What's the problem with the this table?

The first thing is that there's no `primary key`. That is for now there's no attribute that can uniquely identify each row.

As for the repeating group, there are [different meanings](https://qr.ae/pvj0QM) of the repeating group:

Here let's talk about `multi-value attributes`. I'm using a new table to talk about this:

![](https://i.imgur.com/crDIKTU.png)

As we can see, there are many `multi-value attributes`.

For the employee name, we may have first name, last name and middle name; for the children, one employee can have no or many children.

![](https://i.imgur.com/MfvJS2l.png)

For the name, we can divide it like this.

But can we divide the `Children` and `Children Birthday` like this?

![](https://i.imgur.com/JqhvRb9.png)

We cannot do this, because firstly there will be **a lot of null values**, then how can we deal with employees that have **5 children or more**?

So we should do like this:

![](https://i.imgur.com/62vuv1b.png)

Let's go back to the `primary key` of the original table.

![](https://i.imgur.com/hrtbzzZ.png)

As no attribute can uniquely identify each row, can we use `composite key` as `primary key`?

Sure! if we use `StudentNo` with `CourseNo`, then we can uniquely identify each row.

and the dependency should be 
`StudentNo -> StudentName, Address`
`CourseNo -> Course`
`{StudentNo, CourseNo} -> StudetName, Address, Course`

After we add the primary key, then this table is in `1NF`.

### Moving to 2NF

![](https://i.imgur.com/hrtbzzZ.png)

Still the same table, can we say this table is in `2NF` with a primary key `{StudentNum, CourseNum}`?

No, because there's `partial dependencies`.

As we have shown:

`StudentNo -> StudentName, Address`
`CourseNo -> Course`
`{StudentNo, CourseNo} -> StudetName, Address, Course`

`CourseNum` is part of the primary key `{StudentNum, CourseNum}`, so the partial dependency exists.

So if we do not have `composite key` and the table is already in `1NF`, can we say this table is automatically in `2NF`? I would say "yes".

Then how do we go to 2NF?

Firstly, we break them into 2 tables:

![](https://i.imgur.com/EnRPLvE.png)

Looks better? We just removed the `partial dependency` to a new table.

But previously they are linked together, how do we do that?

I would say that depends on the relationship.

Between `Student` and `Course`, typically that should be a `many-to-many relationship`.

That is, one student can register many courses; one course can have many students.

So we should add an associate table to link them together:

![](https://i.imgur.com/ePdHyKl.png)

In which, `StudentNo` and `CourseNo` are `foreign keys`.

### Moving to 3NF

Is the previous table in `3NF`?

Yes, because there's no `transtive dependencies`.

So how can we suppose to do something?

We make a little change to the previous table.

![](https://i.imgur.com/VvKpwHP.png)

Do you think this table is in `3NF`?

I would say no.

Because of the FD below:

If we take `StudentNo` as `primary key`, then:

`StudentNo -> SIN, Student Name, Address`

`SIN -> Student Name, Address`

This may not be a good example, but it can some how describe many things.

1). Can we use `SIN` as `primary key`? 

Yes, but we can only choose one of them.

2). Why can't we use both `StudentNo` and `SIN` as `primary key`?

Because `primary key` is the minimum subset of `super key`.

If we can take `{StudentNo, SIN}` as `primary key`, why don't we take all the attributes?

That's because the properties of `primary key`:

First, `primary key` is indexed and we do not often (or cannot) change it. If we make unnecessary attributes to be the primary key, when we are going to change any of them, there will be troubles.

Second, `primary key` must not be null, so that's also why you always only want a "small enough" primary key.

So let's go back to the point.

There's a transitive dependency exists:

`StudentNo -> SIN`, `SIN -> Student Name, Address`

So how do we change to 3NF?

We do it in the same way: to break it up.

We move all attributes in the `transitive dependency` to a new table.

![](https://i.imgur.com/EngRJvJ.png)

That's it.
