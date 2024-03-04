<template>
  <div class="home d-flex flex-column align-items-center">
    <h2>
      Simulador sorteo Libertadores 2024
    </h2>
    <p>(Cuando se definan las fases previas actualizo)</p>
    <div
      class="d-flex justify-content-center flex-wrap"
      style="max-width: 800px"
    >
      <div
        v-for="(bombo, index) in clubsBase"
        :key="index"
        class="border w-50 mb-2"
      >
        <h4 class="pb-1 pt-1 border-bottom">
          Bombo {{ index + 1 }}
        </h4>
        <div class="d-flex flex-column">
          <span
            v-for="club in bombo"
            class="d-flex align-items-start px-2 justify-content-between"
            :key="club.name"
            :title="club.name == 'Huracan' ? '🤏' : ''"
          >
            <span class="d-flex align-items-start">
              <img
                style="max-width: 18px"
                :src="getImgUrl(club.name, '.png', 'clubes')"
                alt=""
                :title="club.name == 'Huracan' ? '🤏' : ''"
              >
              <b class="mx-1">
                {{ club.name }}
              </b>
            </span>
            <div>
              <img :src="getImgUrl(club.country, '.gif', 'flags')" alt="">
            </div>
          </span>
        </div>
      </div>
    </div>
    <div class="text-secondary my-2">
      • Se deja constancia de que dentro de un mismo grupo no podrá haber dos equipos de un mismo país, por lo tanto, si resultan sorteados, pasarán a ocupar el siguiente grupo y se sorteará nuevamente otro equipo para dicho grupo.
    </div>


    <div class="my-2">
      <button
        class="btn btn-primary"
        @click="init"
      >
        Simular
      </button>
    </div>

    <div
      class="container-fluid d-flex align-items-center justify-content-center"
    >
      <div
        v-if="!loading"
        class="d-flex flex-wrap"
        style="max-width: 800px"
      >
        <div
          v-for="group in 'ABCDEFGH'.split('')"
          :key="group"
          class="d-flex w-50"
        >
          <div class="w-100 m-1 border d-flex flex-column mb-1">
            <b class="py-1 w-100">
              Grupo {{ group }}
            </b>
            <div class="d-flex flex-column">
              <span
                v-for="club in groups[group]"
                class="d-flex align-items-start px-2 pt-1 justify-content-between border-top"
                :key="club.club"
              >
                <span
                  class="d-flex align-items-start"
                  :title="club.name == 'Huracan' ? '🤏' : ''"
                >
                  <img
                    :src="getImgUrl(club.name, '.png', 'clubes')"
                    style="max-width: 18px"
                    alt=""
                    :title="club.name == 'Huracan' ? '🤏' : ''"
                  >
                  <b class="mx-1">
                    {{ club.name }}
                  </b>
                </span>
                <div>
                  <img :src="getImgUrl(club.country, '.gif', 'flags')" alt="">
                </div>
              </span>
            </div>
          </div>
        </div>
      </div>
      <div v-else>
        loading
      </div>
    </div>
    <div>
      <h5 class="my-2">
        FASE PREVIA:
      </h5>
      <img src="../assets/fase-previa-cuadro.jpg" alt="" style="width: 780px; max-width: 100%">
    </div>
    <div class="mt-4">
      Para conmebol posting con amor, Lucas Ferrario. Todos los derechos e izquierdos bien puestos.
      <br>
      Tambien, si lo usan Mariano, Bruno o Pablo en sus vivos voy a ser feliz, abrazo muchachos.
    </div>
  </div>
</template>

<script>
import { cloneDeep } from 'lodash'

export default {
  name: 'HomeView',
  components: {
  },
  data () {
    return {
      retries: 0,
      loading: true,
      groups: {},
      clubsBase: [
        [
          {
            name: 'Fluminense',
            country: 'BR'
          },
          {
            name: 'Palmeiras',
            country: 'BR'
          },
          {
            name: 'River',
            country: 'AR'
          },
          {
            name: 'Flamengo',
            country: 'BR'
          },
          {
            name: 'Gremio',
            country: 'BR'
          },
          {
            name: 'Penarol',
            country: 'UY'
          },
          {
            name: 'Sao Paulo',
            country: 'BR'
          },
          {
            name: 'Liga de Quito',
            country: 'EC'
          }
        ],
        [
          {
            name: 'Atl. Mineiro',
            country: 'BR'
          },
          {
            name: 'Ind. del Valle',
            country: 'EC'
          },
          {
            name: 'Libertad',
            country: 'PA'
          },
          {
            name: 'Cerro Porteno',
            country: 'PA'
          },
          {
            name: 'Estudiantes (LP)',
            country: 'AR'
          },
          {
            name: 'Barcelona',
            country: 'EC'
          },
          {
            name: 'Bolivar',
            country: 'BO'
          },
          {
            name: 'Junior',
            country: 'CO'
          }
        ],
        [
          {
            name: 'San Lorenzo',
            country: 'AR'
          },
          {
            name: 'The Strongest',
            country: 'BO'
          },
          {
            name: 'Universitario (P)',
            country: 'PE'
          },
          {
            name: 'Dep. Tachira',
            country: 'VE'
          },
          {
            name: 'Rosario Central',
            country: 'AR'
          },
          {
            name: 'Alianza Lima',
            country: 'PE'
          },
          {
            name: 'Millonarios',
            country: 'CO'
          },
          {
            name: 'Talleres',
            country: 'AR'
          }
        ],
        [
          {
            name: 'Caracas',
            country: 'VE'
          },
          {
            name: 'Liverpool',
            country: 'UY'
          },
          {
            name: 'Huachipato',
            country: 'CH'
          },
          {
            name: 'Cobresal',
            country: 'CH'
          },
          {
            name: 'Botafogo-Bragantino',
            country: 'XX'
          },
          {
            name: 'Nacional (P)-Palestino',
            country: 'XX'
          },
          {
            name: 'Always Ready-Nacional (U)',
            country: 'XX'
          },
          {
            name: 'Sp. Trinidense-Colo colo',
            country: 'XX'
          }
        ]
      ],
      clubsClone: []
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    init () {
      this.retries = 0
      this.clubsClone = cloneDeep(this.clubsBase)
      this.groups = {}

      this.getGroups()
    },
    getGroups () {
      this.clubsClone.forEach(clubs => {
        'ABCDEFGH'.split('').forEach(group => {
          if (!this.groups[group]) {
            this.groups[group] = []
          }
          console.log(clubs)

          if (this.groups[group].length == 4) {
            return
          }
          this.getClubForGroup(this.groups[group].length, this.groups[group])
        })
      })
      this.loading = false
    },
    getClubForGroup (from, group) {
      if (this.retries >= 10) {
        this.init()
        return
      }
      let current = this.getRandomFrom(this.clubsClone[from])

      if (!group.find(x => x.country == current.country)) {
        group.push(current)
        this.clubsClone[from] = this.clubsClone[from].filter(x => x.name != current.name)
      } else {
        this.retries++
        this.getClubForGroup(from, group)
      }
    },
    getRandomFrom(where) {
      return where[Math.floor(Math.random() * where.length)]
    },
    getImgUrl(img, ext, where) {
      return require('../assets/'+where+'/'+img+ext)
    }
  }
}
</script>
