<template>
  <div class="p-5 bg-blue-100 border-round-xl m-5 flex flex-column justify-content-start align-content-center h-screen">
    <DataTable :value="customers">
      <Column field="customerName" header="Müşteri adı"></Column>
      <Column field="customerEmail" header="Müşteri e-mail adresi"></Column>
      <Column field="customerPhone" header="Müşteri telefonu"></Column>
    </DataTable>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import { getFirestore, query,collection,getDocs } from 'firebase/firestore';



export default {
  name: 'CustomerManagement',
  setup() {
    const customerList = ref([]);
    const db = getFirestore();

    const getCustomerData = async () => {
      const q = query(collection(db, "customers"));
      await getDocs(q).then((querySnapshot) => {
        querySnapshot.forEach((item) => {
          customerList.value.push(item.data());
          console.log(customerList.value)
        });
      });

    }
    onMounted(() => {
      getCustomerData();
    })

    return {
      customerList,
    };
  }
};
</script>