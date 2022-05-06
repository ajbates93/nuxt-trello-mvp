<script>
import { workspaceList } from '../../store/global.js'
export default {
  setup() {
    return {
      workspaceList
    }
  },
  data: () => ({
    workspaceName: '',
    board: {
      name: 'Test',
      columns: []
    },
    newCardItem: ''
  }),
  methods: {
    createColumn() {
      this.board.columns.push({
        newItemName: '',
        items: []
      })
    },
    createCard(column) {
      if (column.newItemName) {
        column.items.push({
          id: 123,
          name: column.newItemName
        })
        column.newItemName = ''
      }
    }
  },
  mounted() {
    this.workspaceName = this.workspaceList.find(
      workspace => workspace.id === Number(this.$route.params.id
    )).name
  }
}
</script>

<template>
  <div>
    <h1>{{ workspaceName }} Workspace (#{{ $route.params.id }})</h1>
    <section>
      <h2>{{ board.name }}</h2>
      <button @click="createColumn">Create Column</button>
      <div class="column-grid">
        <section class="board-column" v-for="column in board.columns" :key="column.id">
          <input type="text" v-model="column.newItemName" @keyup.enter="createCard(column)" />
          <button @click="createCard(column)">Create Card</button>
          <ul>
            <li v-for="item in column.items" :key="item.id">
              {{item.name}}
            </li>
          </ul>
        </section>
      </div>
    </section>
  </div>
</template>

<style>
.column-grid {
  display: grid;
  grid-template-columns: repeat(v-bind(board.columns.length), 1fr);
}
.board-column {
  border: 1px solid #222;
  height: 80vh;
  width: 100px;
}
.board-column input {
  width: 100%;
  box-sizing: border-box;
}
</style>