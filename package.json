var express = require('express')
var bodyParser = require('body-parser')
var app = express()
var http = require('http').Server(app)
var io = require('socket.io')(http)
var mongoose = require('mongoose')


/**
 var options = {
    apiVersion: 'v1', // default
    endpoint: 'http://127.0.0.1:8200', // default
    token: '1234' // optional client token; can be fetched after valid initialization of the server
  };

  // get new instance of the client
var vault = require("node-vault")(options);


// init vault server
vault.init({ secret_shares: 1, secret_threshold: 1 })
.then( (result) => {
  var keys = result.keys;
  // set token for all following requests
  vault.token = result.root_token;
  // unseal vault server
  return vault.unseal({ secret_shares: 1, key: keys[0] })
})
.catch(console.error);


*/

 /**
  * vault.write('secret/hello', { value: 'world', lease: '1s' })
.then( () => vault.read('secret/hello'))
.then( () => vault.delete('secret/hello'))
.catch(console.error);
  */

app.use(express.static(__dirname))
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({extended: false}))

var dbURL = 'mongodb://51.141.45.107:50000/node-demo'

var Message = mongoose.model('Message',{name : String , message: String})


app.get('/messages', (req, res) =>{
   Message.find({},(err, messages) => {
    res.send(messages)
   })
    
})

app.post('/messages', (req, res) =>{
    var message = new Message(req.body)

    message.save((err) => {
        if(err)
        res.sendStatus(500)     


        io.emit('message',req.body);
        res.sendStatus(200)
    })

   
})

io.on('connection', (socket) => {
    console.log('a user connected')
})

mongoose.connect(dbURL, { useMongoClient: true }, (err) => {
    console.log('mongo db connection', err)
})
var server = http.listen(8888, () => {
    console.log('server is listening on port', server.address().port)
})
