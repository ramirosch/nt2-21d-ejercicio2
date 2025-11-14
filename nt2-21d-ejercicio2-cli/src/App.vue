<template>
  <div class="container-fluid mt-3">
    <h2>Listado de Personas</h2>

    <div class="row mb-3">
      <div class="col-md-6">
        <label for="filtro-nombre">Filtrar por Nombre/Apellido</label>
        <input type="text" id="filtro-nombre" class="form-control" v-model="filtroNombre" placeholder="Mínimo 3 caracteres">
      </div>
      <div class="col-md-6">
        <label for="filtro-dni">Filtrar por DNI</label>
        <input type="text" id="filtro-dni" class="form-control" v-model="filtroDni" placeholder="Mínimo 3 caracteres">
      </div>
    </div>

    <div v-if="mostrarAlerta" class="alert alert-warning" role="alert">
      ⚠️ Por favor, ingresa al menos **3 caracteres** en el filtro activo.
    </div>
    
    <div class="row mt-3">
        <div class="col-sm-6 col-md-4 col-lg-3" v-for="persona in filteredPersonas" :key="persona.dni">
          <div class="card mb-3">
            <div class="card-body">
              <h5 class="card-title">{{getNombreCompleto(persona)}}</h5>
              <p class="card-text">DNI: {{persona.dni}}</p>
              <a :href="`mailto:${persona.correo}`" class="card-link">{{persona.correo}}</a>
            </div>
          </div>
        </div>
        
        <div class="col-12" v-if="filteredPersonas.length === 0 && !mostrarAlerta">
           <div class="alert alert-info mt-3" role="alert">
              No se encontraron resultados que coincidan con los filtros ingresados.
          </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filtroNombre: '', 
      filtroDni: '',    
      personas: [
        { nombre: "Daniel", apellido: "Sanchez", correo: "danielsanchez68@hotmail.com", dni: "20442873" },
        { nombre: "Juan", apellido: "Perez", correo: "j@p.gmail.com", dni: "12345678" },
        { nombre: "Ana", apellido: "Suarez", correo: "a@s.gmail.com", dni: "87654321" },
        { nombre: 'Carlos', apellido: 'Lopez', correo: 'c@l.com', dni: '11223344' },
        { nombre: 'Pedro', apellido: 'Rodriguez', correo: 'p@r.com', dni: '98765432' },
      ]
    }
  },

  computed: {
    mostrarAlerta() {
        const nombreF = this.filtroNombre.trim();
        const dniF = this.filtroDni.trim();
        
        const nombreActivo = nombreF.length > 0;
        const dniActivo = dniF.length > 0;

        const nombreInvalido = nombreActivo && nombreF.length < 3;
        const dniInvalido = dniActivo && dniF.length < 3;
        
        return (nombreActivo && nombreInvalido) || (dniActivo && dniInvalido);
    },

    filteredPersonas() {
      const nombreF = this.filtroNombre.trim().toLowerCase();
      const dniF = this.filtroDni.trim();

      const nombreValido = nombreF.length === 0 || nombreF.length >= 3;
      const dniValido = dniF.length === 0 || dniF.length >= 3;

      if (!nombreValido || !dniValido) {
          return nombreF.length > 0 || dniF.length > 0 ? [] : this.personas;
      }

      return this.personas.filter(persona => {
        let coincideNombre = true;
        let coincideDni = true;

        if (nombreF.length >= 3) {
            const nombreCompleto = `${persona.nombre} ${persona.apellido}`.toLowerCase();
            coincideNombre = nombreCompleto.includes(nombreF);
        }

        if (dniF.length >= 3) {
            coincideDni = persona.dni.includes(dniF);
        }

        return coincideNombre && coincideDni;
      });
    }
  },

  methods: {
    getNombreCompleto(persona) {
      return `${persona.nombre} ${persona.apellido}`
    }
  }
}
</script>

<style scoped>
</style>