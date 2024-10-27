# React Material UI

pre-requisities - HTML, CSS, Javascript, Basics of React.js

## What is MUI?

Material-UI (MUI) is a popular open-source design library for React applications. It provides a set of reusable UI components and styles that help you create modern, responsive, and visually appealing user interfaces.

## Environment setup

- code editor: VSCode
- install: node
- install material ui `npm install @mui/material @emotion/react @emotion/styled`

## Typography

- deals with text styling (font-size, font-weight)
- 13 variants: h1...h6, subtitle1, subtitle2, body1(better for paragraph, default one; font size 16px; it is the default value for the variant), body2 (better for paragraph), button,caption,overline
- component props will help us to change the tag h1
- gutterBottom add margin at the bottom

## Button

- variant props: text(less attention, card footer), contained(grab more attention; primary attraction - register/login),outlined (in between - secondary aaction: go back button)

## Let's build a simple project

1. create a react project and install material ui
   `npm create vite@latest`
   `npm install @mui/material @emotion/react @emotion/styled`
2. create a container -> `<Container>` props can be fixed or maxWidth="sm"
3. add typography

   ```ts
   <Container fixed>
     <Typography variant="h4" align="center">
       Ali Express
     </Typography>
   </Container>
   ```

4. get some data and render products

   - `https://fakestoreapi.com/products`

   ```tsx
   const products = [];

   <Container fixed>
     <Typography variant="h4" align="center">
       Ali Express
     </Typography>
     {products.length > 0 &&
       products.map((product) => {
         return (
           <article key={product.id}>
             <img src={product.image} alt={product.title} />
             <h2>{product.id}</h2>
             <p>{product.title}</p>
             <p>{product.description}</p>
           </article>
         );
       })}
   </Container>;
   ```

5. add Card

   ```tsx
   return (
     <Container fixed>
       <Typography variant="h4" align="center">
         Ali Express
       </Typography>
       {products.length > 0 &&
         products.map((product) => {
           return (
             <Card key={product.id}>
               <CardMedia
                 component="img"
                 alt={product.title}
                 height="140"
                 image={product.image}
               />

               <CardContent>
                 <Typography variant="h5">{product.title}</Typography>
                 <Typography variant="body2">{product.description}</Typography>
                 <Button variant="outlined" color="primary">
                   Add to Cart
                 </Button>
               </CardContent>
             </Card>
           );
         })}
     </Container>
   );
   ```

6. add grid -> grid can be parent and child

   - we add container props to the parent
   - we add item props to the parent
   - we can add bgcolor='primary.light', p={2}

```tsx
<Grid spacing={2} container>
  {products.map((product) => {
    return (
      <Grid item xs={6} md={4} lg={3}>
        <Card key={product.id}>
          <CardMedia
            component="img"
            alt={product.title}
            height="140"
            image={product.image}
          />

          <CardContent>
            <Typography variant="h5">{product.title}</Typography>
            <Typography variant="body2">{product.description}</Typography>
            <Button variant="outlined" color="primary">
              Add to Cart
            </Button>
          </CardContent>
        </Card>
      </Grid>
    );
  })}
</Grid>
```

7. how to customize the theme

- create the theme and make the changes -> theme.tsx

  ```tsx
  // theme.js
  import { createTheme } from '@mui/material/styles';

  const theme = createTheme({
    palette: {
      primary: {
        main: '#1976D2', // Change the primary color to blue
      },
    },
    typography: {
      fontFamily: 'Arial, sans-serif',
    },
    // Add more customizations here
  });

  export default theme;
  ```

- provide the theme

```tsx
ReactDOM.render(
  <ThemeProvider theme={theme}>
    <App />
  </ThemeProvider>,
  document.getElementById('root')
);
```

8. customize theme with makeStyles
9. e-commerce app designing
   Let’s extend the layout to include a **Sidebar**, **Pagination**, and update the **Product List** with pagination support. I’ll also show where you can fetch paginated data from an API to integrate this with a back-end later if needed.

   ### **Updated Folder Structure**
   Here’s the updated structure:
   
   ```
   /src
     /components
       Navbar.js
       Footer.js
       ProductCard.js
       Sidebar.js
       Pagination.js
     /pages
       HomePage.js
       ProductDetails.js
       SignUp.js
       SignIn.js
     App.js
     index.js
   ```
   
   ### **Code Updates for Each Component**
   
   #### **Sidebar.js**
   
   The sidebar could be used for category filters, price filters, etc.
   
   ```jsx
   import React from 'react';
   import { Box, Typography, List, ListItem, ListItemText } from '@mui/material';
   
   const Sidebar = () => {
     const categories = ["Electronics", "Fashion", "Home Appliances", "Books", "Toys"];
   
     return (
       <Box sx={{ width: 250, p: 2, borderRight: "1px solid #ddd" }}>
         <Typography variant="h6">Categories</Typography>
         <List>
           {categories.map((category, index) => (
             <ListItem button key={index}>
               <ListItemText primary={category} />
             </ListItem>
           ))}
         </List>
       </Box>
     );
   };
   
   export default Sidebar;
   ```
   
   #### **Pagination.js**
   
   This component controls the current page and displays pagination controls.
   
   ```jsx
   import React from 'react';
   import { Pagination } from '@mui/material';
   
   const PaginationComponent = ({ page, count, onPageChange }) => (
     <Pagination
       count={count}
       page={page}
       onChange={(_, value) => onPageChange(value)}
       color="primary"
     />
   );
   
   export default PaginationComponent;
   ```
   
   #### **Updated HomePage.js**
   
   Integrate the Sidebar and Pagination components into the home page, including pagination handling.
   
   ```jsx
   import React, { useState, useEffect } from 'react';
   import { Grid, Container, Box } from '@mui/material';
   import Sidebar from '../components/Sidebar';
   import ProductCard from '../components/ProductCard';
   import PaginationComponent from '../components/Pagination';
   
   const products = [
     { id: 1, title: "Product 1", price: "$10", imageUrl: "/images/product1.jpg" },
     { id: 2, title: "Product 2", price: "$15", imageUrl: "/images/product2.jpg" },
     { id: 3, title: "Product 3", price: "$20", imageUrl: "/images/product3.jpg" },
     { id: 4, title: "Product 4", price: "$25", imageUrl: "/images/product4.jpg" },
     { id: 5, title: "Product 5", price: "$30", imageUrl: "/images/product5.jpg" },
     { id: 6, title: "Product 6", price: "$35", imageUrl: "/images/product6.jpg" },
     // More products as needed
   ];
   
   const PRODUCTS_PER_PAGE = 4;
   
   const HomePage = () => {
     const [page, setPage] = useState(1);
     const [paginatedProducts, setPaginatedProducts] = useState([]);
   
     useEffect(() => {
       const start = (page - 1) * PRODUCTS_PER_PAGE;
       const end = start + PRODUCTS_PER_PAGE;
       setPaginatedProducts(products.slice(start, end));
     }, [page]);
   
     return (
       <Container>
         <Grid container spacing={2}>
           <Grid item xs={3}>
             <Sidebar />
           </Grid>
           <Grid item xs={9}>
             <Grid container spacing={4}>
               {paginatedProducts.map((product) => (
                 <Grid item xs={12} sm={6} md={4} key={product.id}>
                   <ProductCard product={product} />
                 </Grid>
               ))}
             </Grid>
             <Box mt={4} display="flex" justifyContent="center">
               <PaginationComponent
                 page={page}
                 count={Math.ceil(products.length / PRODUCTS_PER_PAGE)}
                 onPageChange={setPage}
               />
             </Box>
           </Grid>
         </Grid>
       </Container>
     );
   };
   
   export default HomePage;
   ```
   
   #### **ProductCard.js** (No Changes Required)
   
   This component will remain the same as previously created.
   
   ### **App.js**
   
   Ensure the sidebar, pagination, and routing are handled correctly in the main App component.
   
   ```jsx
   import React from 'react';
   import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
   import Navbar from './components/Navbar';
   import Footer from './components/Footer';
   import HomePage from './pages/HomePage';
   import ProductDetails from './pages/ProductDetails';
   import SignUp from './pages/SignUp';
   import SignIn from './pages/SignIn';
   
   function App() {
     return (
       <Router>
         <Navbar />
         <Routes>
           <Route path="/" element={<HomePage />} />
           <Route path="/product/:id" element={<ProductDetails />} />
           <Route path="/signup" element={<SignUp />} />
           <Route path="/signin" element={<SignIn />} />
         </Routes>
         <Footer />
       </Router>
     );
   }
   
   export default App;
   ```
   
   ### **Explanation of Key Updates**
   
   1. **Sidebar**: Lists categories, which could later be used for filtering products.
   2. **PaginationComponent**: A reusable pagination component from Material-UI to handle page changes.
   3. **HomePage Updates**: Added sidebar, pagination, and logic to paginate the list of products.
   4. **App Structure**: Incorporates the updated components and routes.
   
   This structure should provide a complete e-commerce layout with proper navigation, filtering, and pagination functionality. You can further expand the functionality with filtering and sorting logic on the sidebar.
11. aa
