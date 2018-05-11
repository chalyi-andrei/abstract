## Single Responsibility Principle

![Single Responsibility Principle image](./assets/sr.jpg)


> A class should have one and only one reason to change, meaning that a class should only have one job.

Now I want to give one Javascript example on this principle:

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
                
            } else ('seller') {
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

The "SignUp" class performs 2 duties, one must take responsibility for sending user data, the second for routing and loading required content. SignUp class should not take the routing and loading responsibility because suppose some days after your customer asked you for the seller you must give the option of selecting the display of information, both for the seller only, or for both the seller and the buyer.
 Then this class will need to be changed and that is not good. 


```javascript
   class Routing extends Component {
       componentDidMount() {
           const { data } = this.props;
           const userData = getSomeInformation(data.role);
       }

       getSomeInformation = async(role) => {
           const response = await axios.get(`/${role}Information`);
           // do somthing
           this.goTo(role);
           return response;
       }

       goTo = (role) => {
        browserHistory.push(`${role}`)
       }

       render() {
           return (
               <Header />
                {
                    this.props.list(l => <Item item={l} />) 
                }
                <Footer />
           )
       }
   } 
```

So according to SRP, one class should take one responsibility so we should write one different class for routing, so that any change in routing should not affect the SignUp class