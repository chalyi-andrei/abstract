## Unit Testing

### Ui testing with Enzime

 ```javascript
import React from 'react';
import { shallow, configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

configure({ adapter: new Adapter() });

// Testing Function
const Test = () => (
  <p>I am Groot</p>
);

describe('<Test />', () => {
  it('render test', () => {
    const wrap = shallow(<Test />);
    console.log('test', wrap.debug())
    expect(wrap.find('p').length).toEqual(1)
  });
});
 ```


### testing with Jest

 ```javascript
import React from 'react';
import { shallow } from 'enzyme';
import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
import axios from 'axios';

configure({ adapter: new Adapter() });


/**************** Testing Components with Enzyme ****************/
const Test = () => (
  <p>I am Groot</p>
);
describe('<Test />', () => {
  it('render test', () => {
    const wrap = shallow(<Test />);
    expect(wrap.find('p').length).toEqual(1);
  });
});


/**************** Testing with Jest ****************/
function sum(a, b) {
  return a + b;
}
function getUser() {
  return { name: 'John', email: 'johndoe@gmail.com', friends: ['James', 'Rihana'] };
}

// Numbers
test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
  expect(sum(1, 2)).not.toBe(0);
  expect(sum(1, 2)).not.toBeUndefined();
  expect(sum(1, 2)).toBeGreaterThan(2);

});

// Array
test('Array friends contain "Rihana"', () => {
  const user = getUser();
  expect(user.friends).toContain('Rihana');
});

// Object
test('getUser has keys', () => {
  expect(getUser()).toHaveProperty('name', 'John');
});

// Async functions
test('Async function getPost has post with id=1', () => {
  expect.assertions(1);
  return axios.get('https://jsonplaceholder.typicode.com/posts/1')
    .then(data => expect(data.data).toHaveProperty('id', 1));
});

// Async await
test('getPost functions has post with id=1 (async await)', async () => {
  expect.assertions(1);
  const data = await axios.get('https://jsonplaceholder.typicode.com/posts/1');
  expect(data.data).toHaveProperty('id', 2);
});


 ```

