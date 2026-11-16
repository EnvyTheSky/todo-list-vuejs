<script>
export default {
  props: {
    tasks: {
      type: Array,
      required: true
    },
  },
  emits: ['update-tasks'],

  methods: {
    deleteTask(e) {
      const rows = [...document.getElementsByClassName('row')]
      const index = rows.indexOf(e.target.parentElement.parentElement.parentElement)
      const copy = [...this.tasks]
      copy.splice(index, 1)
      this.$emit('update-tasks', copy)
    },
    toggleFinished(e) {
      const rows = [...document.getElementsByClassName('row')]
      const currentRow = e.target.parentElement.parentElement
      const index = rows.indexOf(currentRow)
      const copy = [...this.tasks]
      if (copy[index].status === "unfinished") {
        currentRow.classList.remove(copy[index].status)
        copy[index].status = "finished"
      } else {
        currentRow.classList.remove(copy[index].status)
        copy[index].status = "unfinished"
      }
      this.$emit('update-tasks', copy)
    },
    editTask(e) {
      const rows = [...document.getElementsByClassName('row')]
      const index = rows.indexOf(e.target.parentElement.parentElement.parentElement)
      const item = rows[index].querySelector('.to-do-item')
      const inp = document.createElement('input')
      const copy = [...this.tasks]
      inp.value = item.innerHTML
      inp.classList.add('edit-input')
      item.replaceWith(inp)
      inp.focus()
      let value;
      inp.addEventListener('keydown', (e) => {
        if (e.key === 'Enter') {
          value = inp.value
          item.innerHTML = value
          inp.replaceWith(item)
          copy[index].item = value
          this.$emit('update-tasks', copy)
        }
      })
    },
  }
}
</script>

<template>
  <tr v-for="(task, index) in tasks" :key="index" class="row" :class="{ unfinished: task.status === 'unfinished', finished: task.status === 'finished' }">
    <td><div @click="toggleFinished($event)" class="checkbox"></div></td>
    <td>
      <p class="to-do-item">{{ task.item }}</p>
    </td>
    <td>
      <div class="actions">
        <p @click="editTask($event)" class="edit">✏️</p>
        <p @click="deleteTask($event)" class="delete">🗑️</p>
      </div>
    </td>
  </tr>
</template>

<style>
tr {
  border-bottom: 1px solid black;
}

td {
  padding-block: 5px;
}

td:first-of-type {
  width: 44px;
}

.checkbox {
  width: 20px;
  height: 20px;
  border-radius: 100%;
  background-color: white;
  border: 1px solid lightgrey;
  cursor: pointer;
  position: relative;
}

.to-do-item {
  color: var(--blackberry-cream);
  font-size: 1.125em;
}

.actions {
  display: flex;
  justify-content: end;
}

.edit,
.delete {
  cursor: pointer;
}

.finished .checkbox::after {
  content: url(/Check.svg);
  position: absolute;
  top: -5px;
  width: 25px;
  height: 25px;
}

.finished .to-do-item {
  text-decoration: line-through;
}

.edit-input {
    width: 150px;
    height: 30px;
    border: 2px solid var(--blackberry-cream);
}

@media (min-width: 992px) {
  td:first-of-type {
    width: 54px;
  }

  .checkbox {
    width: 30px;
    height: 30px;
  }

  .finished .checkbox::after {
    content: url(/Check-desktop.svg);
    top: -8px;
    left: -2px;
  }
}
</style>