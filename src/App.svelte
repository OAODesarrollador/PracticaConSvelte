<script>
 
 import { Toast } from "bootstrap";

let toastEl;
$: console.log(toastEl);

let opc = {texto: '', color: ''}

let instancia;
$: if(toastEl){
instancia = new Toast(toastEl)
}

const mostrarToast = (texto, color) => {
  opc = {texto, color}
  instancia.show()
}

let todos = [];
let todo = { id: "", texto: "", estado: false };

if (localStorage.getItem("todos")) {
  todos = JSON.parse(localStorage.getItem("todos"));
}

$: localStorage.setItem("todos", JSON.stringify(todos));

  
  
  const addTodo = () => {
    if (!todo.texto.trim()) {
      console.log("No se puede agregar una tarea vacia");
      todo.texto = "";
      return;
    }
    todo.id = Date.now();
    todos = [...todos, todo];
    todo = { id: "", texto: "" , estado: false };
    mostrarToast('Tarea agregada', 'bg-primary');  
  }
  const deleteTodo = (id) => {
	  todos = todos.filter(item => item.id !== id);
    mostrarToast('Tarea eliminada', 'bg-danger');  
  }
  const editTodo = (id) => {
	  todos = todos.map(item => (item.id === id ? { ...item, estado: !item.estado } : item));
    mostrarToast('Tarea editada', 'bg-warning');
  }
  const classIcono = valor => valor ? 'bi bi-arrow-clockwise' : 'bi bi-check2';
  const classEstado = valor	=> valor ? 'btn-success' : 'btn-warning';
</script>

<div class="container">
  <h1 class="display-5 my-3">CRUD con Svelte</h1>
  <form on:submit|preventDefault={addTodo}>
    <input
      type="text"
      bind:value={todo.texto}
      placeholder="Escribe una tarea"
      class="form-control shadow border-0"
    />
  </form>
  {#each todos as item	}
    <div class="shadow my-3 p-3 lead">
      <p class={item.estado ? "text-decoration-line-through" : ""}>{item.texto}</p>
      <button class="btn {classEstado(item.estado)}" on:click={editTodo(item.id)}>
        <i class={classIcono(item.estado)}></i>
      </button>
      <button class="btn btn-danger" on:click={deleteTodo(item.id)}>
        <i class="bi bi-trash"></i>
      </button>
    </div>
  {/each}
  <div class="toast-container position-absolute top-0 end-0 p-3">
    <div bind:this={toastEl} class="toast align-items-center text-white {opc.color} border-0" role="alert" aria-live="assertive" aria-atomic="true">
    <div class="d-flex">
      <div class="toast-body">{opc.texto}</div>
      <button type="button" class="btn-close btn-close-white me-2 m-auto" data-bs-dismiss="toast" aria-label="Close"></button>
    </div>
  </div>
  </div>
  
</div>
