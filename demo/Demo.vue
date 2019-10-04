<template>
  <div>
    <div class="container">
      <float-label label="">
        <input type="text" class="test" name="test1" :placeholder="'test1'">
      </float-label>
      <float-label label="">
        <input type="text" name="test2" :placeholder="'test2'">
      </float-label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'demo',
  data () {
    return {
      email: 'bkzl@me.com',
      version: 'beta',
      first_name: '',
      options: [
        { value: 'alpha', text: 'Alpha' },
        { value: 'beta', text: 'Beta' },
        { value: 'stable', text: 'Stable' },
      ]
    }
  },
  computed: {
    isActive () {
      return false
    }
  }
}
</script>

<style>
.container {
  width: 150px;
  margin: auto;
}

body {
  padding: 2em;
}

input,
textarea,
select {
  margin-bottom: 2em;
  line-height: 1.15;
  font-size: 14px;
  padding: .4em;
  width: 100%;
}

.example .vfl-label {
  text-transform: uppercase;
}

.class {
  padding-right: 30px;
}

.example .vfl-label-on-input {
  top: -1em;
}

.example .vfl-label-on-focus {
  color: #ff851b;
}

.example .vfl-label + input {
  padding-left: 0;
  font-size: 100%;
  border: 0;
  border-bottom: 2px solid #aaa;
  transition: border 0.2s;
}

.example .vfl-label-on-focus + input  {
  border-bottom: 2px solid #ff851b;
}
</style>
