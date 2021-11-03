<template>
  <div style="padding: 20px">
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="" name="first">
        <span slot="label">
          <span
            class="sgz"
            style="
              background: #ccc;
              font-family: emoji;
              font-size: 15px;
              font-weight: 900;
              padding: 4px 8px;
              border-radius: 50%;
            "
            >直</span
          >
          <span class="name"> 用户管理 </span>
        </span>
        <el-form
          :inline="true"
          :model="formInline"
          class="demo-form-inline"
          label-width="120px"
        >
          <el-form-item label="用户昵称">
            <el-input
              v-model="formInline.user"
              placeholder="请输入用户昵称"
            ></el-input>
          </el-form-item>
          <el-form-item label="用户姓名">
            <el-input
              v-model="formInline.user"
              placeholder="请输入用户姓名"
            ></el-input>
          </el-form-item>
          <el-form-item label="状态">
            <el-input v-model="formInline.user" placeholder="审批人"></el-input>
          </el-form-item>
          <el-form-item label="手机号">
            <el-input
              v-model="formInline.user"
              placeholder="请输入手机号"
            ></el-input>
          </el-form-item>
          <el-form-item>
            <el-button icon="el-icon-refresh" type="primary" @click="onSubmit"
              >重置</el-button
            >
            <el-button icon="el-icon-search" type="primary" @click="onSubmit"
              >查询</el-button
            >
          </el-form-item>
        </el-form>
        <el-divider></el-divider>

        <el-row :gutter="20">
          <el-col :span="6" v-for="(item, i) in data" :key="i">
            <el-card class="box-card">
              <div style="display: flex; justify-content: space-between">
                <img
                  style="width: 60px; height: 60px; border-radius: 50%"
                  size="large"
                  :src="item.img"
                />
                <div>
                  <div :style="{ color: item.color, 'text-align': 'right' }">
                    <i
                      :class="item.num > 0 ? 'el-icon-top' : 'el-icon-bottom'"
                    ></i
                    >{{ item.num }}
                    %
                  </div>
                  <div style="color: #999; font-size: 16px; margin-top: 6px">
                    比昨日增长
                  </div>
                </div>
              </div>
              <div style="line-height: 40px; color: #999">{{ item.des }}</div>
              <div style="font-size: 25px">{{ item.num1 }}</div>
            </el-card>
          </el-col>
        </el-row>

        <el-divider></el-divider>

        <div style="display: flex; justify-content: space-between">
          <el-button icon="el-icon-user" size="mini" type="primary"
            >新增用户</el-button
          >
          <el-button icon="el-icon-upload" size="mini">导出</el-button>
        </div>

        <el-table
          :data="tableData"
          border
          style="width: 100%; margin-top: 20px"
        >
          <el-table-column fixed prop="time" label="创建时间" width="160">
          </el-table-column>
          <el-table-column prop="name" label="用户账号" width="100">
          </el-table-column>
          <el-table-column prop="nicheng" label="用户昵称" width="100">
          </el-table-column>
          <el-table-column prop="phone" label="手机号" width="120">
          </el-table-column>
          <el-table-column prop="price" label="账户余额" width="90">
            <template slot-scope="scope">
              <span style="color: #2678ff"
                >{{ scope.row.price }}<i class="el-icon-money"></i
              ></span>
            </template>
          </el-table-column>
          <el-table-column prop="status" label="状态" width="100">
            <template slot-scope="scope">
              <span v-if="scope.row.status == 1" style="color: #2678ff"
                >正常</span
              >
              <span v-else style="color: #f00">不正常</span>
            </template>
          </el-table-column>
          <el-table-column prop="trueName" label="是否实名" width="100">
            <template slot-scope="scope">
              <span
                v-if="scope.row.trueName == '真名'"
                style="color: #2678ff"
                >{{ scope.row.trueName }}</span
              >
              <span v-else style="color: #f00">{{ scope.row.trueName }}</span>
            </template>
          </el-table-column>
          <el-table-column prop="tip" label="备注"> </el-table-column>
          <el-table-column fixed="right" label="操作" width="150">
            <template slot-scope="scope">
              <el-button
                @click="handleClick(scope.row)"
                type="text"
                size="small"
                >查看</el-button
              >
              <el-button
                @click="handleClick(scope.row)"
                type="text"
                size="small"
                >查看上级</el-button
              >
              <el-button type="text" size="small">编辑</el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-pagination
          style="display: flex; justify-content: flex-end; margin-top: 10px"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage4"
          :page-sizes="[100, 200, 300, 400]"
          :page-size="100"
          layout="total, sizes, prev, pager, next, jumper"
          :total="400"
        >
        </el-pagination>
      </el-tab-pane>
      <el-tab-pane label="配置管理" name="second">
        <span slot="label">
          <span
            class="sgz"
            style="
              background: #ccc;
              font-family: emoji;
              font-size: 15px;
              font-weight: 900;
              padding: 4px 8px;
              border-radius: 50%;
            "
            >间</span
          >
          <span class="name"> 间联管理 </span>
        </span>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
// export default {
//   data() {
//     return {
//       activeName: "first",
//       formInline: {
//         user: "",
//         region: "",
//       },
//       data: [
//         {
//           img: require("./img/用户管理 - 间联.png"),
//           num: 20,
//           des: "团队总人数（个）",
//           num1: "98765",
//           color: "#0d9d59",
//         },
//         {
//           img: require("./img/用户管理 - 间联 (1).png"),
//           num: -5,
//           des: "直联团队人数（个）",
//           num1: "98765",
//           color: "#f9800b",
//         },
//         {
//           img: require("./img/用户管理 - 间联 (2).png"),
//           num: 17,
//           des: "间联团队人数（个）",
//           num1: "98765",
//           color: "#3875f6",
//         },
//         {
//           img: require("./img/用户管理 - 间联 (3).png"),
//           num: -2,
//           des: "总余额（元）",
//           num1: "9876,215",
//           color: "#fd2f37",
//         },
//       ],
//       tableData: [
//         {
//           name: "姓名",
//           nicheng: "嘻嘻哈哈",
//           phone: "12345678901",
//           time: "2020-09-90 11:11:11",
//           price: "231",
//           status: "1",
//           trueName: "真名",
//           tip: "手动新增",
//         },
//         {
//           name: "姓名",
//           nicheng: "嘻嘻哈哈",
//           phone: "12345678901",
//           time: "2020-09-90 11:11:11",
//           price: "231",
//           status: "1",
//           trueName: "真名",
//           tip: "手动新增",
//         },
//         {
//           name: "姓名",
//           nicheng: "嘻嘻哈哈",
//           phone: "12345678901",
//           time: "2020-09-90 11:11:11",
//           price: "231",
//           status: "2",
//           trueName: "假名",
//           tip: "手动新增",
//         },
//       ],
//     };
//   },
//   methods: {
    
//   },
// };

@Component({
  name: 'user',
  components: {},
})
export default class extends Vue {
  
  private activeName = "first"
  private formInline = {
        user: "",
        region : "",
      }
  private data = [
        {
          img: require("./img/用户管理 - 间联.png"),
          num: 20,
          des: "团队总人数（个）",
          num1: "98765",
          color: "#0d9d59",
        },
        {
          img: require("./img/用户管理 - 间联 (1).png"),
          num: -5,
          des: "直联团队人数（个）",
          num1: "98765",
          color: "#f9800b",
        },
        {
          img: require("./img/用户管理 - 间联 (2).png"),
          num: 17,
          des: "间联团队人数（个）",
          num1: "98765",
          color: "#3875f6",
        },
        {
          img: require("./img/用户管理 - 间联 (3).png"),
          num: -2,
          des: "总余额（元）",
          num1: "9876,215",
          color: "#fd2f37",
        },
      ]
  private tableData = [
        {
          name: "姓名",
          nicheng: "嘻嘻哈哈",
          phone: "12345678901",
          time: "2020-09-90 11:11:11",
          price: "231",
          status: "1",
          trueName: "真名",
          tip: "手动新增",
        },
        {
          name: "姓名",
          nicheng: "嘻嘻哈哈",
          phone: "12345678901",
          time: "2020-09-90 11:11:11",
          price: "231",
          status: "1",
          trueName: "真名",
          tip: "手动新增",
        },
        {
          name: "姓名",
          nicheng: "嘻嘻哈哈",
          phone: "12345678901",
          time: "2020-09-90 11:11:11",
          price: "231",
          status: "2",
          trueName: "假名",
          tip: "手动新增",
        },
      ]
      private async handleClick(tab:any, event:any) {
      console.log(tab, event);
    }
}
</script>