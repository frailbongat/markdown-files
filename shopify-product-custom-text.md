# Shopify Product Custom Text

## How to have different custom text in each product.

### Go to your shopify code editor.
1. In **Shopify Admin**, Click **Online Store > Themes**.
1. Click **Actions > Edit Code**.

---
---

### Find the code associated with it.
1. The code is located at **product-price.liquid** file.
1. In the **Search Box**, search for **product-price.liquid**.
1. Open the file.
1. In the file, search for **CUSTOM TEXT START**

---
---

### Understanding the code

The code will look like
```jinja
{% case product.id %}   
{% when 2105379389528 %}
    <div style="font-size: 16px !important;color: #000 !important;margin-top: 5px !important;">
        + ✈️ Get FREE shipping worldwide today
    </div>
    <div style="font-size: 16px !important;color: #000 !important;">
        + ⏱ Buy now to get it shipped tomorrow
    </div>
{% when 2020208246872 %}
    <div style="font-size: 16px !important;color: #000 !important;margin-top: 5px !important;">
        + ✈️ Get FREE shipping worldwide today
    </div>
    <div style="font-size: 16px !important;color: #000 !important;">
        + ⏱ Buy now to get it shipped tomorrow
    </div>
{% when 2109370466392 %}
    <div style="color: #6d6d6d; font-size: 14px;">
        No description yet...
    </div>
{% else %}
	<div style="color: #6d6d6d; font-size: 14px;">
        No description yet...
    </div>  
{% endcase %}
```

---

To simplify,
```jinja
{% case product.id %}
{% when 2105379389528 %}
    YOUR PRODUCT DESCRIPTION
{% when 2020208246872 %}
    YOUR PRODUCT DESCRIPTION
{% else %}
    YOUR DESCRIPTION
{% endcase %}
```

---

Each product corresponds with this block of code
```jinja
{% when 2105379389528 %}
    YOUR PRODUCT DESCRIPTION
```
* `2105379389528` is the **product id**.
* Each **product** has its own **product id**.
* The idea is that, `YOUR PRODUCT DESCRIPTION` will only **render**, if the **product page id that your are visiting** is equal to the **product id** on the code.

---
This is the default, which means `YOUR DESCRIPTION` will **render**, if you don't put a specific description to a new product
```jinja
{% else %}
    YOUR DESCRIPTION
```

---
---

### To get the product id
1. In **Shopify Admin**, Click **Products > All products**.
1. Click on the **product** that you want to get the **id**.
1. Notice that the **URL** looks like this

    `https://drop-and-drive-holder.myshopify.com/admin/products/2105379389528`
1. You can get the **product id** at the end of the **URL**.
1. The product id looks like `2105379389528`

---
---

### To add a specific description to that product
1. Go to the **code**. *(as mention above)*
1. **Before this block of code:**
    ```jinja
    {% else %}
        YOUR DESCRIPTION
    ```
1. **Insert this block of code**:
    ```jinja
    {% when 2105379389528 %}
        YOUR PRODUCT DESCRIPTION
    ```
1. **Change** `2105379389528` into the **product id of your desired product**.
1. Insert your **product description** by replacing `YOUR PRODUCT DESCRIPTION`. *(Hint: You can add HTML & CSS here)*
1. Hit **save**.