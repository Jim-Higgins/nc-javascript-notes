#### Testing endpoints using `supertest`
​
> **"Tests must ALWAYS be written first"** - _most good back-end developers, ever_
​
`supertest` should be installed as a _dev-dependency_ (`npm i -D supertest`), as it is only used for testing, and will not be required for production code - i.e. when the api server is hosted.
​
`supertest` should be required in at the top of the relevant `spec` file as `request`, along with the express `app` that is being tested.
​
To make a test request to the app, the `request` variable (supertest) is passed the `app` as an argument within the mocha `it` block - if the server is not already listening for connections then it is bound to a temporary port, so there is no need to keep track of ports.
​
The HTTP method is then chained on to `request` and invoked with the relevant `path`. E.g:
​
```js
request(app).get('/api/houses');
```
​
`supertest` then has some built in assertions than can be chained on to the request to check the response's HTTP `status` code, as well as any header `field`s and `value`s. E.g:
​
```js
request(app)
  .get('/api/houses')
  .expect(200)
  .expect('Content-Type', 'application/json');
```
​
The above returns a promise, which allows the chaining of `.then()` to access the response from the server. And within this `.then()` block, additional assertions can be made using `chai` as to what we `expect` the body of the response to contain:
​
```js
request(app)
  .get('/api/houses')
  .expect(200)
  .then((res) => {
    expect(res.body.houses).to.have.length(4);
  });
```
​
**The request must be returned** inside the `it` block's callback function in order for the assertions to be run and the mocha tests to correctly pass/fail.
​
Without returning the request, the mocha test would **always** appear to pass.
​
```js
// spec/app.spec.js
const { expect } = require('chai');
const request = require('supertest');
const app = require('../app');
​
describe('/api', () => {
  describe('/houses', () => {
    it('GET returns status 200 & houses object containing an array of the houses', () => {
      return request(app)
        .get('/api/houses')
        .expect(200)
        .then(({ body: { houses } }) => {
          expect(houses).to.have.length(4);
          expect(houses[0]).to.eql({
            house_id: 1,
            house_name: 'Gryffindor',
            founder: 'Godric Gryffindor',
            animal: 'Lion',
          });
        });
    });
  });
});
```