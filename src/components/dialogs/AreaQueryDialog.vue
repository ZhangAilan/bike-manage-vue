<template>
    <div class="area-query-dialog">
        <p>这里是关于投放区域的详细信息。您可以查看各个区域的单车分布情况、区域热度等。</p>    
        <table class="area-table">
        <thead>
            <tr>
            <th>编号</th>
            <th>名称</th>
            <th>容量</th>
            <th>当前存量</th>
            <th>警告等级</th>
            <th>操作</th>
            </tr>
        </thead>
        <tbody></tbody>
        </table>
    </div>
  </template>
  
  <script>
  export default {
    name: 'AreaQueryDialog',
    methods: {
      closeDialog() {
        this.$emit('close'); // 发出关闭事件
      }
    }
  };
  </script>
  
<style scoped>
.area-query-dialog {
padding: 20px;
}

.area-details {
margin-top: 10px;
}

.area-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.area-table th, .area-table td {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
}

.area-table th {
  background-color: #f1f1f1;
}
</style>