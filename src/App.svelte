<script>
  let toDos = [];
  let toDo = { id: 0, text: "", status: false };

  if(localStorage.getItem("toDos")){
      toDos = JSON.parse(localStorage.getItem("toDos"))
  }
  $:localStorage.setItem("toDos", JSON.stringify(toDos)) 

  const addToDo = () => {
    if (!toDo.text.trim()) {
      toDo.text = "";
      return;
    }

    toDo.id = Date.now();
    toDos = [...toDos, toDo];
    toDo = { id: 0, text: "", status: false };

  };

  const deleteToDo = id => {
      toDos = toDos.filter(item => item.id !== id)
  }

  const editToDo = id =>{
	  toDos = toDos.map(item => item.id === id ? {...item, status: !item.status} : item)
  }
</script>

<main class="container">
  
  <form on:submit|preventDefault={addToDo}>
	<h1 class="display-5 my-3">To Do's app</h1>
    <input
	  id="changed"
      type="text"
      placeholder="Enter To Do"
      class="form-control shadow border-0"
      bind:value={toDo.text}
    />
	<input id="submit_changed" disabled={!toDo.text} type="submit" class="btn btn-primary mb-4 mt-2" value="Add new Task">
  </form>
  <table id="task_head">
	<tr>
	  <th>Tasks</th>
	  <th>Actions</th>
	</tr>
  {#each toDos as item}

  <tr>
	<td class={item.status ? "text-decoration-line-through" : ""}>{item.text}</td>
	<td>
		<button on:click={editToDo(item.id)} class={item.status ? "btn btn-sm btn-success":"btn btn-sm btn-warning"}>Completed</button>
		<button on:click={deleteToDo(item.id)} class="btn btn-sm btn-danger">Delete</button>
	   </td>
  </tr>
  {/each}

</main>

<style>
 h1{
	 text-align: center;
 }

 #changed{
	 width: 500px;
	 margin-left: 100px;
 }

 #submit_changed{
	 margin-left: 40%;
	 box-shadow: 4px 4px 4px #777;
 }

 #task_head {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#task_head td, #task_head th {
  border: 1px solid #ddd;
  padding: 8px;
}

#task_head tr:nth-child(even){background-color: #f2f2f2;}

#task_head tr:hover {background-color: #ddd;}

#task_head th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04AA6D;
  color: white;
}
</style>