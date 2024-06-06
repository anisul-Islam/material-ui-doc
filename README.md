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
