<template>
  <div>
    <table style="border-collapse: collapse; width: 800px">
      <tbody>
        <tr
          v-for="(rowData, rowIndex) in tableData"
          :key="rowIndex"
        >
          <td
            v-for="(cellData, cellIndex) in rowData"
            :key="cellIndex"
            style="border: 1px solid #dddddd; padding: 2px 8px"
          >
            {{ cellData }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      required: true,
    },
  },
  computed: {
    tableData() {
      return this.buildTableData();
    },
  },
  methods: {
    calculateRowAndColumn() {
      let maxColumn = 0;
      let maxRow = 0;
      this.data.forEach(([coordinates]) => {
        const [x, y] = coordinates;
        maxColumn = Math.max(maxColumn, Math.floor(x / 100));
        maxRow = Math.max(maxRow, Math.floor(y / 50));
      });
      return { rows: maxRow + 1, columns: maxColumn + 1 };
    },
    buildTableData() {
      const { rows, columns } = this.calculateRowAndColumn();
      const tableData = [];
      for (let i = 0; i < rows; i++) {
        tableData.push(Array(columns).fill(""));
      }
      this.data.forEach(([coordinates, content]) => {
        const { row, column } = this.calculateCellPosition(coordinates);
        tableData[row][column] = content;
      });
      return tableData;
    },
    calculateCellPosition(coordinates) {
      const [x, y] = coordinates;
      return { row: Math.floor(y / 50), column: Math.floor(x / 100) };
    },
  },
};
</script>
