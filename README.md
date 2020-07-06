# Style Guide

# Table of contents
1. [Naming](#naming)

## *1. Naming* <a name="naming"></a>

This project uses [BEM](http://getbem.com/) as a naming convention.
 
When a block component is very large, it can be splitted using the corresponding BEM name. For example, `_order.scss` has the following separate fields for `_order--new.scss` and `_order--edit.scss` for the respective 'new' and 'edit' variants.

## *2. SCSS Rules ordering and nesting*

This is the order that each rule in the style-sheets must follow.

 1. Mixins includes
 2. Properties (alphabetically sorted)
 3. Pseudo classes (:hover, :active)
 4. Pseudo elements (::before, ::after )
 5. Media Queries
 6. Subelements (alphabetically sorted)
 7. Tags (nested p, h1, etc.)
 8. BEM subelements
 9. BEM modifiers
 10. Parent dependent styles

Note that a sub-element must apply this recursively. See and study the following example:

```scss
.block {  
  @include button-text;  
  @include button-general;  
  color: white;  
  padding: 20px 0;  
  width: 100%;  
  // pseudo classes  
  &:hover {  
    /* some rules */  
  }  
  // pseudo elements  
  &::before {  
    /* some rules */  
  }  
  // media queries  
  @media (min-width: $screen-sm-min) {  
    /* some rules */  
  }  
  // subelements: tags/classes  
  h1 {  
    /* some rules */  
  }  
  p {  
    @include text-general;  
    color: black;  
    // pseudo elements  
    &:before {  
      /* some rules */  
    }  
    // media queries  
    @media (min-width: $screen-lg-min) {  
      /* some rules */  
    }  
  }  
  .parent-class & {  
    /* some rules */  
  }  
  // subelements: BEM elements  
  @at-root &__subelement {  
    /* some rules */  
  }  
  // subelements: BEM modifiers  
  @at-root &--collapsed {  
    /* some rules */  
  }  
}  
```
For cases with only one property, please check following example and comments inline:

```scss
.block {  
  @include button-general;  
  @include button-text;  
  h1 {  
    /* some rules */  
  }  
  // subelements: BEM elements  
  @at-root &__items {  
    display: inline-block;  
    // subelements: tags  
    a {  
      color: $white;  
    }  
  }  
}  
```

## *3. Class order in markup*

Structural classes must follow the following order:

Block / Block sub-element 
Modifiers  

Example:

```jsx
<div className="feature-box feature-box--highlighted">  
  <div className="feature-box__image feature-box__image--rounded">...</div>  
</div> 
```

If you are mixing classes from `antd` with structural classes on the same elements the ones corresponding to `antd` must be placed first:

antd specific classes  / Block / Block sub-element  
Modifiers

```jsx
<div className="ant-rate-text feature-box feature-box--highlighted">...</div>
```

## *4. Targeting elements in rules*

Always favor to target elements using its classes unless you are very sure that you won't affect another element of the same type. For example, to target the following list:

```jsx
<div className="parent">  
  <ul className="main-menu">....</ul>  
</div>
```  

Always try to use:

```scss
.parent {  
 .main-menu { /* some properties */}  
}
```

Avoiding this as much as possible:

```scss
.parent {  
  ul { /* some properties */}  
}  
```

If the element doesn't have a identifying class, try to add it modifying the markup.

## *5. Naming structural blocks*

Some tips about how to proceed when naming the website elements:

 1. Take a little time to think the structure of the whole block you
    need  to achieve before starting to write markup or styles.
 2. Make a draft and discuss it with your team mates trying to make it
    match or fit in the following suggestions.
    
 3. Evaluate if this block can be reused in the future and think about
    its structure. E.g. start with very basic definition and add more
    custom styles by using BEM modifiers.

Example: 
```scss
.related {  
  // subelements: BEM elements  
  .related__card { 
    /*...*/ 
  }  
  // subelements: BEM modifiers  
  .related--links {  
    .related__card { /* specific styles for related links cards */ }  
  }  
  .related--products {  
    .related__card { /* specific styles for related product cards */ }  
  }  
}
```
    
Try to use up to two levels for BEM elements. Leave the third level for particular cases where including the third level is somehow useful. A good way to determine the nesting level of the component is to think if it could be moved one level up without sacrificing meaning or creating structural inconsistencies. 

Take a look to the `.product__picture` element in the following example. It could be moved to the same level than `.product__body` without much problem and naming it `.product__body__picture` is less flexible and a little bit redundant. 

```scss
.product {  
  .product__body {  
    // subelements: tags/classes  
    .category { /*...*/ }  
    .description { /*...*/ }  
  }  
  .product__header { /*...*/ }  
  .product__picture {  
    .description { /*...*/ }  
    .picture { /*...*/ }  
  }  
} 
```
```jsx
<div className="product">  
  <div className="product__header">...</div>  
  <div className="product__body">  
    <div className="category">...</div>  
    <div className="product__picture">  
      <div className="description">...</div>  
      <img src="img.png" alt="Alt text" className="picture" />  
    </div>  
    <div className="description">...</div>  
  </div>  
</div> 
```

Avoid nesting more than 3 levels as much as possible. E.g. **don't use** `.product__header__title__chapter`. In these cases, using child elements without BEM naming should be enough (see the example).

```scss
.product {  
  .product__header {  
    .product__header__title {  
      .chapter { /*...*/ }  
      .text { /*...*/ }  
      .icon { /*...*/ }  
    }  
  }  
} 
```

Try to create one word components as much as possible. For example, if there are no previous blocks named commodity and this word it's meaningful enough then naming a block just commodity would be the right decision. If you later need to create some variants you could apply BEM modifiers.  

```scss
.commodity {  
  /* main commodity styles */  
  .commodity--product { /* specific styles for products */ }  
  .commodity--services { /* specific styles for services */ }  
}  
```

Auxiliary BEM elements: sometimes we need an element to wrap things or for another reasons and we don't want these elements affect the BEM nesting flow. In these cases it's allowed to use an "auxiliary" BEM element. To mark this special subelements we'll use ---. E.g.  

```jsx
<div class="block">  
  <div class="block---wrapper">  
    <div class="block__subelement"></div>  
  </div>  
</div>
```
