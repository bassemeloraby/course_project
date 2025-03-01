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

# Web Frontend_Backend MERN Project axios 3-2

- create utils folder
- create index.jsx file

https://www.npmjs.com/package/axios

npm i axios

> > index.jsx

import axios from 'axios';

const productionUrl = 'http://localhost:5000/api';

export const customFetch = axios.create({
baseURL: productionUrl,
});

==============================================

# Web Frontend_Backend MERN Project products page 3-3

> > Products.jsx

import { customFetch } from "../utils";

const url = "/products";

export const loader = async () => {
const response = await customFetch(url);
const products = response.data;
console.log(products);
return { products };
};

> > App.jsx

// loaders
import { loader as productsLoader } from "./pages/Products";

loader: productsLoader

- create ProductsList.jsx

> > components/index.js

export { default as ProductsList } from "./ProductsList";

> > Products.jsx

 <div>
      <ProductsList />
    </div>

> > ProductsList.jsx

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

# Web Frontend_Backend MERN Project react-toastify 3-4

https://www.npmjs.com/package/react-toastify

npm i react-toastify

> > main.jsx

import "react-toastify/dist/ReactToastify.css";

import { ToastContainer } from "react-toastify";

<ToastContainer position="top-right" />

> > Products.jsx

if (products.length === 0) {
toast.error("No products found!");
} else {
toast.success("Products loaded successfully!");
console.log(products);
return { products };
}

====================================================

# Web Frontend_Backend MERN Project Single Product Page 3-5

> > SingleProduct.jsx

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

---

import { toast } from "react-toastify";

if (!product) {
toast.error("No product found!");
} else {
toast.success("Single Product is loaded !");
console.log(product);
return { product };
}

> > App.jsx

import { loader as singleProductLoader } from "./pages/SingleProduct";

loader: singleProductLoader

====================================================

# Web Frontend_Backend MERN Project Create Product Page 3-6

> > Products.jsx

<section>
        <Link to="/create-product" className="btn btn-primary capitalize">
          create
        </Link>
      </section>

- create ProductCreate.jsx page
- add to index.js

>> App.jsx

// actions
import { action as createProductAction } from "./pages/ProductCreate";

{
  path: "/create-product",
  element: <ProductCreate />,
  action: createProductAction,
},

- Create FormInput.jsx Component
- add to index.js

>> FormInput.jsx

// eslint-disable-next-line react/prop-types
const FormInput = ({ label, name, type, defaultValue, size }) => {
  return (
    <div className="form-control">
      <label htmlFor={name} className="label">
        <span className="label-text capitalize">{label}</span>
      </label>
      <input
        type={type}
        name={name}
        defaultValue={defaultValue}
        className={`input input-bordered ${size}`}
      />
    </div>
  );
};
export default FormInput;

------------------
- Create SubmitBtn.jsx Component
- add to index.js

>> SubmitBtn.jsx

import { Fragment } from "react";
import { useNavigation } from "react-router-dom";

// eslint-disable-next-line react/prop-types
const SubmitBtn = ({ text }) => {
  const navigation = useNavigation();
  const isSubmitting = navigation.state === "submitting";

  return (
    <button
      type="submit"
      className="btn btn-primary btn-block"
      disabled={isSubmitting}
    >
      {isSubmitting ? (
        <Fragment>
          <span className="loading loading-spinner"></span>
          sending...
        </Fragment>
      ) : (
        text || "submit"
      )}
    </button>
  );
};
export default SubmitBtn;


--------
>> ProductCreate.jsx

import { Form, redirect } from "react-router-dom";
import { FormInput, SubmitBtn } from "../components";
import { customFetch } from "../utils";
import { toast } from "react-toastify";


const url = "/products";

// eslint-disable-next-line react-refresh/only-export-components
export const action = async ({ request }) => {
  const formData = await request.formData();
  const data = Object.fromEntries(formData);
  try {
    const response = await customFetch.post(url, data);
    console.log(response);
    toast.success("New product is added successfully");
    return redirect("/");
  } catch (error) {
    const errorMessage =
      error?.response?.data?.error?.message ||
      "please double check your credentials";

    toast.error(errorMessage);
    return null;
  }
};


 <section className="h-screen grid ">
      <Form
        method="post"
        className="card w-96  p-8 bg-base-100 shadow-lg flex flex-col gap-y-4"
      >
        <h4 className="text-center text-3xl font-bold capitalize">create</h4>
        <FormInput type="text" label="Trade Name" name="tradeName" />
        <FormInput type="text" label="Price" name="price" />
        <div className="mt-4">
          <SubmitBtn text="create" />
        </div>
      </Form>
    </section>

====================================================

# Web Frontend_Backend MERN Project Delete Product 3-7

>> SingleProduct.jsx

 <section>
        <button onClick={deleteHandler} className="btn btn-primary capitalize">
          delete
        </button>
      </section>


const navigate = useNavigate()

  const deleteHandler = async () => {
    try {
      const response = await customFetch.delete(`${url}/${product._id}`);
      console.log(response)
       toast.success("product is deleted successfully");
       return navigate("/");
    } catch (error) {
      const errorMessage =
        error?.response?.data?.error?.message;

      toast.error(errorMessage);
      return null;
    }
    
  };

======================================================
# Web Frontend_Backend MERN Project Update Product Page 3-8

- create UpdateProduct.jsx file in pages
- add to index.jsx file in pages

>> App.jsx




