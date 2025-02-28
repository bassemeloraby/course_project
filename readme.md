# Web Frontend_Backend MERN Project concurrently 3-1

npm init -y

https://www.npmjs.com/package/concurrently

npm i concurrently

create .gitignore

node_modules


"dev": "concurrently \"npm run server\" \"npm run client\"",
    "server": "npm run dev --prefix server",
    "client": "npm run dev --prefix client"
===================================
# Web Frontend_Backend MERN Project  axios 3-2

- create utils folder
- create index.jsx file


https://www.npmjs.com/package/axios

npm i axios

>> index.jsx

import axios from 'axios';

const productionUrl = 'http://localhost:5000/api';

export const customFetch = axios.create({
  baseURL: productionUrl,
});


==============================================
# Web Frontend_Backend MERN Project  products page 3-3



>> Products.jsx

import { customFetch } from "../utils";


const url = "/products";

export const loader = async () => {
  const response = await customFetch(url);
  const products = response.data;
  console.log(products);
  return { products };
};

>> App.jsx

// loaders
import { loader as productsLoader } from "./pages/Products";


 loader: productsLoader

- create ProductsList.jsx

>> components/index.js

export { default as ProductsList } from "./ProductsList";


>> Products.jsx

 <div>
      <ProductsList />
    </div>


 >> ProductsList.jsx


import { Link, useLoaderData } from "react-router-dom";


   const { products } = useLoaderData();



   <div className="mt-12 grid gap-y-8">
      {products.map((product) => {
        const { _id,tradeName, price } = product;
         return (
           <Link
             key={_id}
             to={`/products/${_id}`}
             className="p-8 rounded-lg flex flex-col sm:flex-row gap-y-4 flex-wrap  bg-base-100 shadow-xl hover:shadow-2xl duration-300 group"
           >
             <div className="ml-0 sm:ml-16">
               <h3 className="capitalize font-medium text-lg">{tradeName}</h3>
               <h4 className="capitalize text-md text-content">
                 {price}
               </h4>
             </div>
           </Link>
         );
      })}
    </div>

====================================================

# Web Frontend_Backend MERN Project  react-toastify 3-4


https://www.npmjs.com/package/react-toastify

npm i react-toastify

>> main.jsx

import "react-toastify/dist/ReactToastify.css";

import { ToastContainer } from "react-toastify";

<ToastContainer position="top-right" />


>> Products.jsx


 if (products.length === 0) {
    toast.error("No products found!");
  } else {
    toast.success("Products loaded successfully!");
    console.log(products);
    return { products };
  }

====================================================

# Web Frontend_Backend MERN Project  Single Product Page 3-5

>> SingleProduct.jsx

import { customFetch } from "../utils";
import { useLoaderData, Link } from "react-router-dom";

const url = "/products";

export const loader = async ({ params }) => {
  const { id } = params;
  console.log(id);

  const response = await customFetch(`${url}/${id}`);
  const product = response.data;
  console.log(product);
  return { product };
};


 <section>
      <div className="text-md breadcrumbs">
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/products">Products</Link>
          </li>
        </ul>
      </div>
      {/* PRODUCT */}
      <div>
        <h1 className="capitalize text-3xl font-bold">{tradeName}</h1>
        <h4 className="text-xl text-content font-bold mt-2">
          {price}
        </h4>
      </div>
    </section>


----
import { toast } from "react-toastify";


if (!product) {
      toast.error("No product found!");
    } else {
      toast.success("Single Product is loaded !");
      console.log(product);
      return { product };
    }


>> App.jsx

import { loader as singleProductLoader } from "./pages/SingleProduct";

loader: singleProductLoader






   






