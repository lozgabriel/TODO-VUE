<template>
  <div class="task" :class="stateClass" @click="$emit('taskStateChanged', task)">
    <!-- .Stop irá cancelar a chamade de taskStateChanged-->
    <span @click.stop="$emit('taskDeleted', task)" class="close">x</span>
    <p>{{ task.name + (task.pending === false ? ': Finalizado' : ': Pendente')}}</p>
  </div>
</template>

<script>
export default {
    props: {
        task: { type: Object, required: true}
    },
    computed: {
        stateClass() {
            return {
                pending: this.task.pending,
                done: !this.task.pending
            }
        }
    }
}
</script>

<style>
    .task {
        position: relative;
        box-sizing: border-box;
        width: 350px;
        height: 150px;
        padding: 10px;
        border-radius: 8px;
        font-size: 2rem;
        font-weight: 300;
        cursor: pointer;
        user-select: none;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .pending {
        border-left: 12px solid #b73229;
        background-color: #F44336;
    }

    .done {
        border-left: 12px solid #0a8f08;
        background-color: #4caf50;
    }

    .pending .close {
        background: #b73229;
    }

    .done .close {
        background: #0a8f08;
    }

    .close {
        position: absolute;
        right: 10px;
        top: 10px;
        font-size: 0.9rem;
        font-weight: 600;
        width: 20px;
        height: 20px;
        border-radius: 10px;
        text-align: center;
    }
</style>