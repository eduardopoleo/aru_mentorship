# About
This is an empirical guide based on my experience and little tidbits of knowledge that I've collected over the years. While this guide is oversimplified I would like to think that is mostly accurate. With that said, I would not use it as an authoritative source :).

# The Web

It's a network of computers that communicate with each other by sending information (packets) through interconnected networks. The most common mental model to think about the web is the [OSI model](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/). Each level on the model represents a layer of abstraction and deals with different concerns.

<img title="a title" alt="Alt text" src="/osi-model-7.jpeg">

For our discussion we just need to know that most applications connected to the internet commonly run on top of UDP or TCP which are implemented in the transport layer of the OSI model.

- UDP. It's fire and forget it's used mostly when performance is of the essence and missing a package or two is not a big deal. A good use case for this are real time multiplayer video games where servers and clients need to sync real time data about thousands of objects simultaneously at almost instance speed. Because updates are so frequent missing a package about a player position is not the end of world and in most cases some clients are well equipped to do interpolations when things like this happen.

- TCP. It's less performant than UDP but more reliable. It's used for top level protocols such as http where obtaining the full message is essential. In a web application the neither the client or the server can make any assumptions about the requests that is being made so a full delivery of the message is necessary for the request to be completed.

# HTTP
These lower level protocols form the basis of higher level protocols such http. HTTP is probably the most ubiquitous protocol when it comes to web application and it's built specifically on top of TCP (although http3 which is not widely used yet is implemented on top of UDP). [HTTP](https://en.wikipedia.org/wiki/HTTP/3) is a request/response method and it has 3 main components:

- Verb or Method: In practice there are only 2 methods GET and POST. GET requests are used when retrieving information while POST are used when executing side effects such as adding or changing records, creating background jobs etc.
- URL: Normally, a string sent along with the VERB that allows the server locate the part of the code that should handle the request
- STATUS: Send as part of the response indicates the status of the request

Normally, a computer known as the client initiates a requests by sending a VERB and a URL and other payload required to complete the request. The server which is a computer with a web server that's constantly listening for request  parses the url and the verb from the request and uses it locate the part of the code that should handle the request. It then proceeds to process the payload and return a response along with a CODE indicating the status of the response. Successful GET requests tend to return a 200 error.

A good way to visualize how a http response work is by using curl along with the -v option. In the command line you can try:

`curl https://catfact.ninja/fact -v`

This will print out all the headers and additional information for both request and response. HTTP is a very versatile protocol that can be used to send many different type of files some of them include:
  - Documents: html, xml etc
  - Static files: JS, CSS
  - Media: images and videos maybe?

You can clearly see this by opening a web inspector after loading almost any page in the internet.

<img title="a title" alt="Alt text" src="/http_requests.png">

# Web Sockets
Websockets is a top level protocol but contrary to http it keeps a long live connection that allows both computer to continuously send messages without the need of the request response cycle, so it's ideal for real time application such as chats. Chat GPT gives a good explanation on [websockets](https://chatgpt.com/share/de8231aa-f904-4c0f-a230-b009f945d3df)

[TODO] Build a chat app with rails to showcase this.

# PSQL
PSQL is another application layer protocol and it's mainly used to send database requests (queries) to a database. Again chat GPT has a [good explanation](https://chatgpt.com/share/630921ee-6867-477f-a2c7-344d94fc6117)  the main differences between http and PSQL.

# The Browser

As I mentioned above a regular http response requires two different actors:
  - server -> the computer running a web server which is actively waiting for a response
   - client -> the computer requesting the information to the server
A computer can be both server and client and there are literally billions of devices connected and communicating this way on a daily basis. Among these there is one kind of devices that is very special, the personal computer that communicate with the internet through a browser.
The Browser is essentially "multi compiler" that can connect to the internet and request all these different type of files but the most important ones are HTML, CSS, Javascript.

## HTML
It's markup language consisting of tags and it's main function is to give structure to the site. Effectively, with HTML you be creating the container of all the information that will be displayed in the browser. Then you'll use CSS to position and style them.

## CSS
Consists of cascading set of rules used to arrange and style the contents of the page. For example it can be used to determine the orientation of divs on the page or the background color of the page.

## JS
It's a scripting language used to imprint interactivity onto the website, and in many use cases (such as SPAs) to make direct http queries to server and update the DOM accordingly.

# Web Evolution
  - Initially most of the web was static, meaning that content on the pages could only be changed through deployments. The Web Master needed to upload a new version of the site if something needed to change
  - Not long after ppl started figuring out that they could hook their site to databases and start generated dynamic data. I think this was a logical step since databases already existed it was just a matter of writing the bit of glue code needed to get thing working (do not quote me on this)
  - The web proliferated and user adoption grew but there was another problem for every single interaction that the user wanted to make a full round trip for everything. 
    - An example: if an user filled out an long form and sent it to the server, the server could respond with a validation error and an empty form making the user to loose all the work. In this case it would have been easier to just be able to have a bit of logic to validate the form in the browser.
  - AJAX! The interaction did not stop there and in 2004 ajax was born which added a way to make full on request to the server directly from javascript and modify the contents of the page without having to do a full page request. This opened a new new world of posibilities for instance
    - having a full page refresh for say a like in a post makes for a very jarring experience. In tis particular example you could also save work in the server because the only thing that you needed to take care of was to account for the like. As opposed to having to recalculate the entire page again.
  - At this point the web was becoming the de facto medium to share information and browsers the main gateway and having control of this gateway meant big business $$$. Naturally, there were many competitors trying to take over the browser market. Each of this companies (netscape, MS, etc) had their own browser implementation which came with their own particularities, one specifically annoying was the difference between their JS runtime. Enter JQuery.
  - JQuery revolutionalized the web. Not only gave use a much easier query interface to interact with DOM elements (insert example) but it also dealt with most of the browser implementation details developers did not want to worry about. To this day JQuery is the most popular JS library use to add interactivity to sites despite react, svelte etc...
  - TBC