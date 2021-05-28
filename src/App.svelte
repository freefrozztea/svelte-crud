<script>
  import { v4 as uuidv4 } from "uuid";
  import Noty from "noty";

  import "noty/lib/noty.css";
  import "noty/lib/themes/nest.css";

  let products = [
    {
      id: uuidv4(),
      name: "HP Pavilion Notebook",
      description: "HP Laptop",
      category: "laptop",
    },
    {
      id: uuidv4(),
      name: "Mouse Razer",
      description: "Gaming mouse",
      category: "peripherials",
    },
  ];

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: "",
  };

  let editStatus = false;

  const cleanProduct = () => {
    product = {
      id: "",
      name: "",
      category: "",
      description: "",
      imageURL: "",
    };
  };

  const addProduct = () => {
    const newProduct = {
      id: uuidv4(),
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL,
    };
    products = products.concat(newProduct);
  };

  const updateProduct = () => {
    let updatedProduct = {
      id: product.id,
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL,
    };
    const index = products.findIndex((p) => p.id === product.id);
    products[index] = updatedProduct;
    editStatus = false;
    new Noty({
      theme: "sunset",
      type: "success",
      timeout: 3000,
      text: "Product Updated Successfully",
    }).show();
  };

  const onSubmitHandler = (e) => {
    if (!editStatus) {
      addProduct();
    } else {
      updateProduct();
    }
    cleanProduct();
  };

  const deleteProduct = (id) => {
    products = products.filter((product) => product.id !== id);
  };

  const editProduct = (productEdited) => {
    product = productEdited;
    editStatus = true;
  };
</script>

<main>
  <div class="container p-4">
    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="card mt-2">
            <div class="row">
              <div class="col-md-4 p-2">
                {#if !product.imageURL}
                  <img
                    src="./images/no_products_found.png"
                    alt=""
                    class="img-fluid p-4"
                  />
                {:else}
                  <img
                    src={product.imageURL}
                    alt=""
                    class="img-fluid py-4 pl-4"
                  />
                {/if}
              </div>
              <div class="col-md-8">
                <h5 class="pt-4">
                  <strong>{product.name}</strong>
                  <span>
                    <small>{product.category}</small>
                  </span>
                </h5>
                <p class="card-text">{product.description}</p>
                <button
                  class="btn btn-danger"
                  on:click={deleteProduct(product.id)}
                >
                  Delete
                </button>
                <button
                  class="btn btn-secondary"
                  on:click={editProduct(product)}
                >
                  Edit
                </button>
              </div>
            </div>
          </div>
        {/each}
      </div>
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form on:submit|preventDefault={onSubmitHandler}>
              <div class="form-group">
                <input
                  bind:value={product.name}
                  type="text"
                  placeholder="Product Name"
                  id="product-name"
                  class="form-control"
                />
              </div>
              <div class="form-group">
                <textarea
                  bind:value={product.description}
                  id="product-description"
                  rows="3"
                  placeholder="Product Description"
                  class="form-control"
                />
              </div>
              <div class="form-group">
                <input
                  bind:value={product.imageURL}
                  type="url"
                  id="product-image-url"
                  placeholder="https://github.com"
                  class="form-control"
                />
              </div>
              <div class="form-group">
                <select
                  id="category"
                  bind:value={product.category}
                  class="form-control"
                >
                  <option value="laptops">Laptops</option>
                  <option value="perifherials">Peripherials</option>
                  <option value="servers">Servers</option>
                </select>
              </div>

              <button class="btn btn-primary">
                {#if !editStatus}Save Product{:else}Update Product{/if}
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
