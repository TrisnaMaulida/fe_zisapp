<template>
  <div>
    <Breadcrumbs title="Mustahik" />
    <!-- Container-fluid starts-->
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-12">
          <div class="card">
            <div class="card-header">
              <h5>Data Mustahik</h5>
            </div>
            <div class="card-body">
              <!-- TAMBAHIN KONTENYA DISINI -->
              <data-table
                :items="data"
                :headers="headers"
                :hidesimpan="true"
                :hidecetak="true"
                :hidekonfirm="true"
                :hidedetail="true"
                @add="$router.push({ path: '/main/mustahik/add' })"
                @edit="editMustahik"
                @delete="onDelete"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Container-fluid Ends-->
  </div>
</template>

<script>
import API from "@/services/api.service";
export default {
  data: () => {
    return {
      headers: [
        { text: "Kode", value: "kode_mustahik" },
        { text: "Nama", value: "nama_mustahik" },
        { text: "Asnaf", value: "asnaf" },
        { text: "Kategori", value: "kategori_mustahik" },
        { text: "AKSI", value: "action" },
      ],
      data: [],
    };
  },
  created() {
    this.getData();
  },
  methods: {
    getData() {
      API.get("/api/mustahik").then(({ status, data }) => {
        console.log(data);
        if (status == 200 || status == 201) {
          // reponse dari be jika berhasil

          if (data.status) {
            //berhasil
            this.data = data.data;
          } else {
            //notifikasi gagal
          }
        } else {
          //notifikasi gagal
        }
      });
    },
    editMustahik(data) {
      this.$router.push({ path: "/main/mustahik/edit/" + data.id_mustahik });
    },
    onDelete(data) {
      this.$swal({
        text: this.$t("Delete Message", { who: "" }),
        showCancelButton: true,
        confirmButtonText: "Anda yakin ingin menghapus?",
        confirmButtonColor: "#4466f2",
        cancelButtonText: "Batal",
        cancelButtonColor: "#efefef",
        reverseButtons: true,
      }).then(({ value }) => {
        if (value) {
          API.delete(`/api/mustahik/${data.id_mustahik}`).then(
            ({ status, data }) => {
              if (status == 200 || status == 201) {
                // reponse dari be jika berhasil

                if (data.status) {
                  //berhasil
                  this.$toasted.show("Data Berhasil Dihapus", {
                    theme: "bubble",
                    position: "top-right",
                    type: "success", //"success" kalau su
                    duration: 2000,
                  });
                } else {
                  //notifikasi gagal
                  this.$toasted.show("Data Gagal Dihapus", {
                    theme: "bubble",
                    position: "top-right",
                    type: "error", //"success" kalau su
                    duration: 2000,
                  });
                }
                this.getData();
              } else {
                //notifikasi gagal
                this.$toasted.show("Data Gagal Dihapus", {
                  theme: "bubble",
                  position: "top-right",
                  type: "error", //"success" kalau su
                  duration: 2000,
                });
              }
            }
          );
        }
      });
    },
  },
};
</script>
