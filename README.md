import React from 'react';
import { loadStripe } from "@stripe/stripe-js";
import {
   Elements,
   CardElement,
   useStripe,
   useElements, 
} from "@stripe/react-stripe-js";
import axios from 'axios'

import "bootswatch/dist/lux/bootstrap.min.css";
import './App.css';

const stripePromise = loadStripe("pk_test_51Jirb5JERUQ8HqUEO8TZsDpXGuCcs7rvvF4NMjgERqWWNhl32HTRzUsjiOC3hY3OR0dfEgSUcBpPyZq9Yh8NALDk00Ee9Qb6A1")

const Ckeckoutform = () => {
  const stripe = useStripe();
  const elements = useElements();

  const handlesubmit = async (e) => {
    e.preventDefault();

  const {error, paymentMethod} = await stripe.createPaymentMethod({
   type: 'card',
   card: elements.getElement(CardElement)
  })

  if (!error) {
    const { id } = paymentMethod;

    const {data} = await axios.post('http://Localhost:3001/api/checkout', {  
     id,
     amount: 10000
    })
  
  console.log(data)
  }
  };

return (
 <form onSubmit={handlesubmit} className="card card-body">
  
  <img src="https://www.corsair.com/corsairmedia/sys_master/productcontent/CH-9102020-NA-K68_01.png" 
  alt="k68 keywords" 
  className="img-fluid"
  />

<h3 className="text-center my-2">Precio: 100$</h3>

   <div className="form-group">
      <CardElement className="form-control" />
   </div>

   <button className="btn btn-success">Comprar</button>
 </form>
  );
};

function App() {
  return (
     <Elements stripe={stripePromise}>
       <div className="container p-4">
          <div className="center">
            <div className="coL-md-4 offset-md-4">
              <Ckeckoutform/>
            </div>
          </div>
       </div>
     </Elements>
  );
}

export default App;
