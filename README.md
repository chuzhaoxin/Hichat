HiChat
===
 
A chat application built with Node.js and socket.io.

First we can start to create a simple HTTP server.
Similar to the very simple code below, it creates an HTTP server and listens to port 80 of the system.
Save it as a js file, such as server.js

A package that uses sockets in Node.js. It can be used to easily establish a socket connection from the server to the 
client, send events and receive specific events.
Also install npm install socket.io through npm. We can find a socket.io.js file. It is introduced to the HTML page, 
so that we can use socket.io to communicate with the server on the front end.
<script src="/socket.io/socket.io.js"></script>

At the same time, server.js on the server side, like express, also needs to be introduced into the project through
 require ('socket. io '), so that socket. io can be used on the server side.
When socket. io is used, its front and back end syntax is consistent. That is, an event is fired through socket.On () 
to listen and handle the corresponding event. The two events communicate through the parameters passed. 

The specific working mode can be seen in the following example

IndexHTML: 
            
                <input id="sendBtn" type="button" value="SEND">


hichat.js:  

                document.getElementById('sendBtn').addEventListener('click', function() {
                    var messageInput = document.getElementById('messageInput'),
                        msg = messageInput.value,
                        //font color
                        color = document.getElementById('colorStyle').value;
                    messageInput.value = '';
                    messageInput.focus();
                    if (msg.trim().length != 0) {
                        that.socket.emit('postMsg', msg, color);
                        that._displayNewMsg('me', msg, color);
                        return;
                    };
                }
        
To this end, we can write in server.js as follows:       
server.js:  

               socket.on('postMsg', function(msg, color) {
                   socket.broadcast.emit('newMsg', socket.nickname, msg, color);
               });    

               
hichat.js:  
             
                    
             this.socket.on('newMsg', function(user, msg, color) {
                         that._displayNewMsg(user, msg, color);
                     });
                   
             _displayNewMsg: function(user, msg, color) {
                     var container = document.getElementById('historyMsg'),
                         msgToDisplay = document.createElement('p'),
                         date = new Date().toTimeString().substr(0, 8),
                         //determine whether the msg contains emoji
                         msg = this._showEmoji(msg);
                     msgToDisplay.style.color = color || '#000';
                     msgToDisplay.innerHTML = user + '<span class="timespan">(' + date + '): </span>' + msg;
                     container.appendChild(msgToDisplay);
                     container.scrollTop = container.scrollHeight;
                 },              
        



Features
---
* send pictures :sunrise:
* send emojis :smile:
* keyboard support :musical_keyboard:
* online users count statistic :ghost:

How to run
---
2. run `npm install` 
3. run `node server`
4. finnaly, open your browser and visit `localhost:3000`


