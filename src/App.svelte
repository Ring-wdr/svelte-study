<script>
  let arr = [{id: 1, name: 'kim'}]
  let todoItem = '';
  let changeItem = '';
  let toggle = [];

  const onSubmit = (e) => {
    e && e.preventDefault();
    if (todoItem === '') return alert('값을 입력하세요')
    arr = arr.length > 0
      ? [...arr , {id: arr[arr.length - 1].id + 1, name: todoItem}]
      : [{id: 1, name: todoItem}]
    todoItem = '';
  }
  const changeVisible = (inputId) => {
    toggle = (toggle.some((visible)=> visible === inputId))
      ? toggle.filter((visible)=> visible !== inputId)
      : [...toggle, inputId]
  }
  
  const onModify = (inputId) => {
    arr = arr.map(({id, name})=> inputId === id ? {id, name: changeItem}: {id, name});
    toggle = toggle.filter((visible)=> visible !== inputId)
  }

  const onDelete = (inputId) => {
    arr = arr.filter(({id})=> id != inputId)
  }
</script>

<main>
  <h1>to-do-list</h1>
  <div class="add-todo">
    <form on:submit={onSubmit}>
      <input type="text" name='todo-item' bind:value={todoItem}/>
      <button type="submit">추가</button>
    </form>
  </div>

  <div>
    <ul>
      {#each arr as {id, name}, i}
        <li>{id}: {name}
          <button on:click={()=> onDelete(id)}>삭제</button>
          <button on:click={()=> changeVisible(id)}>수정</button>
          {#if toggle.some((visible) => id === visible) }
            <div>
              <input bind:value={changeItem} />
              <button on:click={()=>onModify(id)}>반영</button>
            </div>
          {/if}
        </li>
      {/each}
    </ul>
  </div>
</main>
