### mongodb transaction

#### init replset mongodb env 

$mongod --port 27017 --dbpath ./db --replSet rstest
$mongod --port 37017 --dbpath ./db --replSet rstest
$mongod --port 47017 --dbpath ./db --replSet rstest

$mongo --port 27017

`
  rs.initiate( {
     _id : "rstest",
     members: [
        { _id: 0, host: "127.0.0.1:27017" },
        { _id: 1, host: "127.0.0.1:37017" },
        { _id: 1, host: "127.0.0.1:47017" },
     ]
  })
`

```
  const mongoose = require('mongoose')
  const mongoUri = 'mongodb://localhost:27017,localhost:37017,localhost:47017/test'
  const client = mongoose.createConnection(mongoUri, { replicaSet: 'rstest' })
  let Acc = client.model('Account', new mongoose.Schema({
      name: String,
      balance: Number
  }))
  async function initTest() {
      await Acc.create({ 'name': 'James', 'balance': 3000 })
      await Acc.create({ 'name': 'Wade', 'balance': 0 })
  }
  async function afterWork() {
      await Acc.remove({ 'name': 'James' })
      await Acc.remove({ 'name': 'Wade' })
  }
  async function transferTest(transfer) {
      let session = await Acc.startSession()
      session.startTransaction()
      try {
          const opts = { session, new: true }
          let a = await Acc.findOneAndUpdate({
              'name': 'James'
          }, {
              $inc: { 'balance': -transfer }
          }, opts)
          console.log(a.toObject());
          let b = await Acc.findOneAndUpdate({
              'name': 'Wade',
          }, {
              $inc: { 'balance': transfer }
          }, opts)
          console.log(b.toObject());
          await session.commitTransaction()
      } catch (err) {
          session.abortTransaction()
          console.error(err)
      } finally {
          session.endSession()
      }

  }
  async function test() {
      await afterWork()
      await initTest()
      await transferTest(100)
          // await afterWork()
  }
  test()
```

set a debug point or sleep point at 
```
  console.log(a.toObject());
```
to stop the process
then run code to read them
`
  { "_id" : ObjectId("5cb752de57e8791edf030e9d"), "name" : "James", "balance" : 3000, "__v" : 0 }
  { "_id" : ObjectId("5cb752de57e8791edf030e9e"), "name" : "Wade", "balance" : 0, "__v" : 0 }

`
it works fine

then run :
`
  rstest:PRIMARY> db.accounts.update({"name":"Wade"}, { $inc:{'balance':10}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

`

this will return immediately, when the debug point continue, it ran into exception

`
  null: MongoError: WriteConflict
  MongoError: Attempted illegal state transition from [TRANSACTION_ABORTED] to [TRANSACTION_ABORTED]
  ....
`