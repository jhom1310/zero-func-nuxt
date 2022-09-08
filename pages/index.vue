<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="12" >
      <v-card>
        <h2 class="text-center">
          CALCULADORA ZERO DE FUNÇÕES
        </h2>
        <br>
        <v-divider />
        <br>
        <h3 class="text-center">
          Tabelamento
        </h3>
        <v-form v-model="valid">
          <v-container>
            <v-row>
              <v-col
                cols="6"
                md="6"
              >
                <v-text-field
                  v-model="funcaoX"
                  :rules="defaultRules"
                  label="Informe uma função"
                  hint="ex: x^3 - 9x +5"
                  persistent-hint
                  required
                  outlined
                />
              </v-col>

              <v-col
                cols="2"
                md="2"
              >
                <v-text-field
                  v-model="amplitude"
                  :rules="defaultRules"
                  label="Amplitude"
                  hint="ex: 0.5"
                  persistent-hint
                  required
                  outlined
                />
              </v-col>
              <v-col
                cols="2"
                md="2"
              >
                <v-text-field
                  v-model="inicio"
                  :rules="defaultRules"
                  label="Inicio"
                  hint="ex: 0"
                  persistent-hint
                  required
                  outlined
                />
              </v-col>
              <v-col
                cols="2"
                md="2"
              >
                <v-text-field
                  v-model="fim"
                  :rules="defaultRules"
                  label="Fim"
                  hint="ex: 2"
                  persistent-hint
                  required
                  outlined
                />
              </v-col>

              <v-col
                cols="12"
                md="6"
              >
              
              <v-container v-if="tabela.length">
                <v-simple-table>
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">
                          X
                        </th>
                        <th class="text-left">
                          F(x)
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        v-for="item in tabela"
                        :key="item.name"
                      >
                        <td>{{ item.x }}</td>
                        <td>{{ item.fx }}</td>
                      </tr>
                    </tbody>
                  </template>
              </v-simple-table> 
            </v-container>
          <v-card-actions>
          <v-spacer />
          <v-btn
            width="100%"
            color="primary"
            nuxt
            @click="tabelamento"
          >
            Gerar Tabelamento
          </v-btn>
        </v-card-actions>
              
              </v-col>
              
            </v-row>
          </v-container>
          <v-divider />
          <br>
          <h4 class="text-center">
            Bissecção
          </h4>
          <v-container>
            <v-row>
              <v-col
                cols="4"
                md="4"
              >
              <v-text-field
                  v-model="bissec_a"
                  :rules="defaultRules"
                  label="Intervalo A"
                  required
                  outlined
                />
              </v-col>

              <v-col
                cols="4"
                md="4"
              >
                <v-text-field
                  v-model="bissec_b"
                  :rules="defaultRules"
                  label="Intervalo B"
                  required
                  outlined
                />
              </v-col>
              <v-col
                cols="4"
                md="4"
              >
                <v-text-field
                  v-model="precisao"
                  :rules="defaultRules"
                  label="Precisão"
                  required
                  outlined
                />
              </v-col>

              <v-container v-if="tabela_bissec.length">
                <v-simple-table>
                  <template v-slot:default>
                    <thead>
                      <tr>
                        <th class="text-left">
                          Iteração
                        </th>
                        <th class="text-left">
                          A
                        </th>
                        <th class="text-left">
                          B
                        </th>
                        <th class="text-left">
                          X
                        </th>
                        <th class="text-left">
                          F(x)
                        </th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        v-for="item in tabela_bissec"
                        :key="item.name"
                      >
                        <td>{{ item.it }}</td>
                        <td>{{ item.a }}</td>
                        <td>{{ item.b }}</td>
                        <td>{{ item.x }}</td>
                        <td>{{ item.fx }}</td>
                      </tr>
                    </tbody>
                  </template>
              </v-simple-table> 
            </v-container>
              
            </v-row>
          </v-container>
        </v-form>

        <v-card-actions>
          <v-spacer />
          <v-btn
            width="100%"
            color="primary"
            nuxt
            @click="bissec"
          >
            Gerar bissecção
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
  import { create, all } from 'mathjs'

const config = { }
const math = create(all, config)
export default {
  auth: 'guest',
  name: 'RegisterPage',

  data: () => ({
    tabela: [],
    tabela_bissec: [],
    tabela_falsa_pos: [],
    valid: false,
    funcaoX: '',
    amplitude: '',
    inicio: '',
    fim: '',
    bissec_a: '',
    bissec_b : '',
    precisao: '',
    defaultRules: [
      v => !!v || 'Campo obrigatório'
    ],
    

  }),
  computed: {
    emailConfirmationRule () {
      return () => (this.email === this.email2) || 'O email deve corresponder'
    },
    passwordConfirmationRule () {
      return () => (this.password === this.password2) || 'A senha deve corresponder'
    }

  },
  watch: {
    cep (newValue, oldValue) {
      this.getCEP()
    }
  },
  methods: {
    async getCEP () {
      await this.$axios.$get('https://viacep.com.br/ws/' + this.cep + '/json/', {
        headers: {
          'Access-Control-Allow-Origin': 'http://localhost:51000/',
          'Access-Control-Request-Method': 'GET',
          Accept: '*'
        }
      }).then((response) => {
        // console.log(response)
        this.endereco = response.logradouro
        this.complement = response.complemento
        this.city = response.localidade
        this.state = response.uf
        this.district = response.bairro
      }).catch()
    },

    tabelamento () {
      //x^3 - 9x +5
      this.tabela = []
      var _x = parseFloat(this.inicio)
      let scope = {
        x: 3,
      }

      for (_x; _x <= parseFloat(this.fim);  _x += parseFloat(this.amplitude)) {
        scope = {
          x: _x.toFixed(6),
        }
        this.tabela.push({x: _x.toFixed(6), fx: math.evaluate(this.funcaoX, scope).toFixed(6)})
        console.log()
        
      }      
    },

    bissec () {
      this.tabela_bissec = []
      var a = parseFloat(this.bissec_a)
      var b = parseFloat(this.bissec_b)
      var precisao = parseFloat(this.precisao)
      let scopeA = {
        x: a,
      }
      let scopeB = {
        x: b,
      }
      let scopeC

      var fa = math.evaluate(this.funcaoX, scopeA)
      var fb = math.evaluate(this.funcaoX, scopeB)
      if (fa * fb <= 0){
        var i = 0
        var c = a
        let scopeC = {x: c}
        
        while(math.abs(math.evaluate(this.funcaoX, scopeC)) >= precisao ){
          c = (a+b)/2
          scopeC = {x: c}
          var fc = math.evaluate(this.funcaoX, scopeC);
          
          this.tabela_bissec.push({a: a.toFixed(6), b: b.toFixed(6), it: i, x: c.toFixed(6), fx: fc.toFixed(6)})
          i++
          if(math.evaluate(this.funcaoX, scopeC) == 0.0){
            break
          }
          console.log(c)
          if(fc * fa < 0){
            console.log(c)
            b = c
            scopeB = {x: b}
          }
          else {
            a = c
            scopeA = {x: a}
          }

        }
      }
    }

  }
}

</script>

<style>

</style>
