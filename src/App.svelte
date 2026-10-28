<script>
  import { onMount } from 'svelte';
  import { supabase } from './lib/supabase.js';

  let nama = "Nama";
  let kontak = "Kontak";

  let inputNama = "";
  let inputTelepon = "";

  let daftarKontak = [];
  let editId = null;

  onMount(() => {
    ambilData();
  });

  async function ambilData() {
    const { data, error } = await supabase
      .from('kontak')
      .select('*')
      .order('id', { ascending: true });

    if (error) {
      console.error('Gagal ambil data:', error.message);
      return;
    }
    daftarKontak = data;
  }

  async function handleSubmit() {
    if (editId === null) {
      const { error } = await supabase
        .from('kontak')
        .insert({ nama: inputNama, telepon: inputTelepon });

      if (error) {
        console.error('Gagal tambah:', error.message);
        return;
      }
    } else {
      const { error } = await supabase
        .from('kontak')
        .update({ nama: inputNama, telepon: inputTelepon })
        .eq('id', editId);

      if (error) {
        console.error('Gagal update:', error.message);
        return;
      }
      editId = null;
    }

    nama = inputNama;
    kontak = inputTelepon;
    inputNama = "";
    inputTelepon = "";

    await ambilData();
  }

  function editKontak(item) {
    inputNama = item.nama;
    inputTelepon = item.telepon;
    editId = item.id;
  }

  async function hapusKontak(id) {
    const { error } = await supabase
      .from('kontak')
      .delete()
      .eq('id', id);

    if (error) {
      console.error('Gagal hapus:', error.message);
      return;
    }

    await ambilData();
  }
</script>

<style>
  h1 { color: orange; }
  h2 { color: cornflowerblue; }

  table {
    border-collapse: collapse;
    margin-top: 20px;
  }

  th, td {
    border: 1px solid #444;
    padding: 8px 16px;
  }

  th { color: orange; }

  .hapus {
    background: #e74c5b;
    color: white;
    border: none;
    padding: 4px 10px;
    cursor: pointer;
  }
</style>

<h1>{nama}</h1>
<h2 id="kontak">{kontak}</h2>

<form on:submit|preventDefault={handleSubmit}>
  <input type="text" bind:value={inputNama} placeholder="Enter your name" required>
  <input type="text" bind:value={inputTelepon} placeholder="Enter your phone number" required>
  <button type="submit">{editId === null ? "Submit" : "Update"}</button>
</form>

<table>
  <thead>
    <tr>
      <th>No</th>
      <th>Nama</th>
      <th>Nomor Telepon</th>
      <th>Aksi</th>
    </tr>
  </thead>
  <tbody>
    {#each daftarKontak as item, i (item.id)}
      <tr>
        <td>{i + 1}</td>
        <td>{item.nama}</td>
        <td>{item.telepon}</td>
        <td>
          <button on:click={() => editKontak(item)}>Edit</button>
          <button class="hapus" on:click={() => hapusKontak(item.id)}>Hapus</button>
        </td>
      </tr>
    {/each}
  </tbody>
</table>