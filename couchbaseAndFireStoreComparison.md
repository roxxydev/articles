# Comparison Between MongoDB and Google Cloud Firestore

## What are MongoDB and Firestore?

Both MongoDB and Google Cloud Firestore databases are NoSQL database. What NoSQL database means is you store data in document-object data structure. In MySQL you have tables while in NoSQL you represent entities as document. A good example of NoSQL databases are MongoDB and Google Cloud Firestore.

To start of with, MongoDB have it's query language that is used to get data while in Google Cloud Firestore you have methods and API calls you can use to retrieve data stored in it. You probably want to choose MongoDB over Firestore if your app requires a fast requests retrieving and writing data. The query performance of MongoDB is quicker compare to Firestore. If you wanted a realtime performance, the Firebase platform provides a service called Real-Time Database. Firestore is originally designed for long-term data storage and retrieval. MongoDB supports ACID (atomicity, consistency, isolation, durability) transactions and in case you're wondering if Firestore also support ACID transactions then the answer is yes.

---

## ACID Characteristic

We'll go through each of these ACID characteristics means. First, in terms of atomicity it's either all or none of the document mutations are applied. For consistency, it means that no transaction will leave the database in an inconsistent state or simply put changes made within transaction are consistent with database constraints and will not violate integrity constraints placed on the data by the database rules. Isolation transactions means that each and every transaction happens in an order without any transactions occurring in tandem and by that it also means that the reads and writes of a separate transaction won't affect the other transaction in that same database. Lastly, the durability ensures that transactions committed to the database will be saved permanently and ensures that data written to database by the committed transaction will not be corrupted in such cases like there's a crash or service failure that occurred in the database server.

All of these ACID characters are both present to MongoDB and Google Cloud Firestore. With MongoDB you can easily switch among service providers or you can host it yourself in your server that you manage. For Google Cloud Firestore since it's within Firebase, you can take advantage of other Firebase services like Google Cloud Functions and the likes for integrating usage with the Firestore. Unlike MongoDB which lets you to self-host on any cloud provider you want like AWS, Firestore is restricted on the Google Cloud Platform because well technically Google owns the Firebase Platform.

---

## Limit and Language

In terms of document size limit, Firestore have a 1MB document size limit while in MongoDB you have a 16MB size limit. We also mentioned earlier that MongoDB used query language called MQL (MongoDB Query Language) and these queries are based on javascript where you have a range of options, operators, expressions and filters. While in Google Cloud Firestore provides a programmatic interface of fetching and writing data in an SQL-like query language.

For example in MongoDB you want to find the first document in the collection, you do

```
db.users.findOne()
```

while in Firestore you do

```
const firestore = getFirestore();
firestore
  .collection("NameOfYourCollection")
  .orderBy("yourTimestamp", "desc")
  .limit(1)
  .get()
  .then(snapshot => {
    snapshot.forEach((doc) => {

        // doc is the first element in your collection
      });
  });
```

You'll see that in Firestore it uses the query like methods provided by their API.

---

## Choosing between the two

There are a lot of factors to consider when choosing what database to use. Things like performance, scalability, maintainability, and pricing. In terms of performance, Firestore comes relatively real-time as it facilitates database synchronization almost in real-time and for it’s scaling Firestore is designed automatically based on user demand. So the more resources it uses the more likely you’ll have to pay. But Firebase provides different pricing plans you can choose from when utilizing Firebase platform services. On the other hand, MongoDB as the likes of other NoSQL database provides horizontal scaling capability. There are different optimization techniques out there which you can utilize when using MongoDB and to name these ways these are `indexing`, `read replicas`, and `sharding`.  When it comes to pricing in MongoDB there’s this provider which name usually comes up which is the MongoDB Atlas where they also provide different pricing plan options.

Overall when choosing what to choose for app development between MongoDB and Firestore, you can fairly say you can go for Google Cloud Firestore if you have only a small project or a mobile application. Firestore will let you store easily, sync data, and let’s you query when needed. Firebase also provides different client sdk library you can use base on what type your application is written. But if you are mind the performance and scale you might want to go for MongoDB at it gives you flexibility to configuring your database to further improve its performance and scaling.
