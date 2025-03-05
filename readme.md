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
# Web Frontend_Backend MERN Project  Product Update Page 3-8

- create ProductUpdate.jsx file in pages
- add to index.jsx file in pages

>> App.jsx

import { loader as UpdateProductLoader } from "./pages/ProductUpdate";
import { action as updateProductAction } from "./pages/ProductUpdate";


 {
        path: "/update-product/:id",
        element: <ProductUpdate />,
        loader: UpdateProductLoader, // use the loader to fetch the product details before rendering the update page
        action: updateProductAction, // use the action to update the product
      },

>> 

 <Link
          to={`/update-product/${product._id}`}
          className="btn btn-success capitalize ms-1"
        >
          edit
        </Link>

>> ProductUpdate.jsx

import { toast } from "react-toastify";
import { customFetch } from "../utils";
import { Form, redirect, useLoaderData } from "react-router-dom";
import { FormInput, SubmitBtn } from "../components";

const url = "/products";

// eslint-disable-next-line react-refresh/only-export-components
export const loader = async ({ params }) => {
  const { id } = params;
  try {
    const response = await customFetch(`${url}/${id}`);
    const product = response.data;

    if (!product) {
      toast.error("No product found!");
      return redirect("/products"); // Redirect to a safe page
    }

    toast.success("Single Product is loaded!");
    return { product };
  } catch (error) {
    console.log(error)
    toast.error("Failed to load product details.");
    return redirect("/products"); // Redirect to a safe page
  }
};

// eslint-disable-next-line react-refresh/only-export-components
export const action = async ({ request, params }) => {
  const formData = await request.formData();
  const data = Object.fromEntries(formData);

  try {
    await customFetch.put(`${url}/${params.id}`, data, {
      headers: {
        "Content-Type": "application/json", // Use JSON for non-file data
      },
    });
    toast.success("Product updated successfully!");
    return redirect(`/products/${params.id}`);
  } catch (error) {
    const errorMessage =
      error?.response?.data?.error?.message ||
      "Please double check your credentials.";
    toast.error(errorMessage);
    return null;
  }
};


const ProductUpdate = () => {
  const { product } = useLoaderData();
  const { tradeName, price } = product;

  return (
    <section className="h-screen grid items-start">
      <Form
        method="post"
        className="card w-96 p-8 bg-base-100 shadow-lg flex flex-col gap-y-4"
      >
        <h4 className="text-center text-3xl font-bold capitalize">
          Update Product
        </h4>
        <FormInput
          type="text"
          label="Trade Name"
          name="tradeName"
          defaultValue={tradeName || ""}
          required
        />
        <FormInput
          type="number"
          label="Price"
          name="price"
          defaultValue={price || ""}
          required
        />
        <div className="mt-4">
          <SubmitBtn text="Update" />
        </div>
      </Form>
    </section>
  );
};

export default ProductUpdate;


======================================================
# Web Frontend_Backend MERN Project Themes 3-9

>> Navbar.jsx

import { BsMoonFill, BsSunFill } from "react-icons/bs";

{/* THEME SETUP */}
            <label className="swap swap-rotate">
              <input type="checkbox" />
              {/* sun icon*/}
              <BsSunFill className="swap-on h-4 w-4" />
              {/* moon icon*/}
              <BsMoonFill className="swap-off h-4 w-4" />
            </label>


onChange={handleTheme}

  const [theme, setTheme] = useState();

  const handleTheme = () => {
    setTheme(!theme);
  };


https://daisyui.com/docs/themes/

https://www.youtube.com/watch?v=vFam7hEZTu8&list=PLhDKyjgkNJZWLEbxiVcAU-LFyEILyzYe8&index=13

>> tailwind.config.cjs

 daisyui: {
    themes: ["winter", "dracula"],
  },


>> Navbar.jsx


const themes = {
  winter: "winter",
  dracula: "dracula",
};

  const [theme, setTheme] = useState(themes.winter);

const handleTheme = () => {
    const { winter, dracula } = themes;
    const newTheme = theme === winter ? dracula : winter;
    document.documentElement.setAttribute("data-theme", theme);

    setTheme(newTheme);
  };


  useEffect(()=>{},[])

useEffect(() => {
    document.documentElement.setAttribute("data-theme", theme);

    localStorage.setItem("theme", theme);
  }, [theme]);

  const getThemeFromLocalStorage = () => {
  return localStorage.getItem("theme") || themes.winter;
};

  const [theme, setTheme] = useState(getThemeFromLocalStorage());

--  
======================================================
# Web Frontend_Backend MERN Project react-redux @reduxjs/toolkit 3-10

https://react-redux.js.org/introduction/getting-started

https://www.npmjs.com/package/react-redux

https://redux-toolkit.js.org/introduction/getting-started

https://www.npmjs.com/package/@reduxjs/toolkit


- npm install react-redux @reduxjs/toolkit
- create app and features and auth folders
- create authSlice.js file
- create store.js file



>> store.js

import { configureStore } from "@reduxjs/toolkit";


export const store = configureStore({
  reducer: {},
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: false,
    }),
});

>> main.jsx

import { store } from "./app/store";
import { Provider } from "react-redux";


<Provider store={store}>
  <App />
  <ToastContainer position="bottom-right" autoClose={1000}/>
</Provider>


>> authSlice.js

import { createSlice } from "@reduxjs/toolkit";

export const authSlice = createSlice({});

export default authSlice.reducer;

const initialState = {
  user: null,
  isError: false,
  isSuccess: false,
  isLoading: false,
  message: "",
};

 name: "auth",
  initialState,
  reducers: {}


>> store.js

import authReducer from "../app/features/auth/authSlice";

auth: authReducer,

-----------------------
redux devtools extension

--------------------

======================================================
# Web Frontend_Backend MERN Project Themes global 3-11

https://www.youtube.com/watch?v=0vtP3SGlqMI&list=PLhDKyjgkNJZW7prwMpN4G_kBohsOCD4Iz&index=8

>> authSlice.js

const themes = {
  winter: "winter",
  dracula: "dracula",
};

const getThemeFromLocalStorage = () => {
  const theme = localStorage.getItem("theme") || themes.winter;
  document.documentElement.setAttribute("data-theme", theme);
  return theme;
};

  theme: getThemeFromLocalStorage(),


 toggleTheme: (state) => {
      const { dracula, winter } = themes;
      state.theme = state.theme === dracula ? winter : dracula;
      document.documentElement.setAttribute("data-theme", state.theme);
      localStorage.setItem("theme", state.theme);
    },

export const { toggleTheme } = authSlice.actions;

>> Navbar.jsx
import { useDispatch, useSelector } from "react-redux";
import { useDispatch } from "react-redux";

  const dispatch = useDispatch();


    dispatch(toggleTheme());
