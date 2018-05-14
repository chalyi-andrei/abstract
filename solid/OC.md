## Open Closed Principle

![Open Closed Principle image](./assets/oc.jpg)

> Objects or entities should be open for extension, but closed for modification.

> Open for extension means that we should be able to add new features or components to the application without breaking existing code.

> Closed for modification means that we should not introduce breaking changes to existing functionality, because that would force you to refactor a lot of existing code

Consider the previous example:

```javascript
   class SignUp extends Component {
       state = {
           name: '',
           password: '',
           role: 'buyer',
       }

       signUp = async(e) => {
            e.preventDefault();
            const { role } = this.state;

            if (role === 'buyer') {
                try {
                    const response = await axios.post('/buyer', { data: this.state });
                    // do somthing with response
                    const response = await axios.get('/buyerInformation');
                    browserHistory.push('/buyer')
                } catch(error) {
                    // handle error
                }
                
            } else if ('seller') {
               // do somthing with seller
            } else {
                // do somthing with admin
            }
       }

       render() {
           const { name, password, role} = this.state;

           return (
                <form onSubmit={(e) => this.signUp(e)}>
                    <input
                        value={name}
                        onChange={e => this.setState({name: e.target.value})}
                    />
                    <input
                        value={password}
                        type="password" 
                        onChange={e => this.setState({password: e.target.value})}
                    />
                    <select value={role} onChange={e => this.setState({role: e.target.value})}>
                        <option>buyer</option>
                        <option>seller</option>
                        <option>admin</option>
                    </select>

                <button>Login</button>
            </form>
           )
       }
   } 
```

The code shows that the SIgnUp class will have to be changed each time we add a new role, or change the current one.

```javascript
    if (role === 'buyer') {
        // do somthing with buyer
    } else if ('seller') {
    // do somthing with seller
    } else {
        // do somthing with admin
    }
```

To avoid this, you can create separate role classes that have their own methods.

```javascript
    
class Buyer {
  getInfo() {
    return axios.get('/buyer');
  }
  goTo() {
    browserHistory.push('products');
  }
  // do somthinc with Buyer
}

class Seller {
  getInfo() {
    return axios.get('/seller');
  }
  goTo() {
    browserHistory.push('home');
  }
  // do somthinc with Seller
}

class Roles {
  constructor () {
    this.buyer = new Buyer();
    this.seller = new Seller();
  }

  // another methods
}

const roles = new Roles();
```

After that, our Roles class will contain all the roles. The call to a particular method will look like this:

```javascript
     signUp = (e) => {
            e.preventDefault();
            const { role } = this.state;

            roles[role].getInfo();
     }
```

Thus, if you need to change the role or add a new one, we will not need to change the class SignUp.
