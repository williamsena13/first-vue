<script>
export default {
    props: [ 'user'
           , 'nome'
           , 'people'
           , 'peopleHealth'
    ],
    data(){
      return {
        list: []
      }
    },
    methods : {
        //******************************************************************
        execute( obj_this ){
          //****************************************************************
          try {

            let obj_user =  (  obj_this.user ? JSON.parse(obj_this.user) : null)
            let obj_people =  (  obj_this.people ? JSON.parse(obj_this.people) : null)
            let obj_peopleHealth = (  obj_this.peopleHealth ? JSON.parse(obj_this.peopleHealth) : null)

            obj_this.populaceForm( obj_user, obj_people, obj_peopleHealth );

            document.getElementById("btn_sus_card").disabled = true;
            document.getElementById("btn_insurance_card").disabled = true;
          } catch(e){
            console.log('>> ERRO AO EXECUTAR FORM-USER <<');
            console.log(e);
          }


        }


        //********************************************************************
      , disabledForm(){
          //*****************************************************************
          let bbo_disabled = ( document.getElementById('btn_edit_form_people').className == 'btn btn-default' );
          try {
            document.getElementById('btn_save_form_people').readyOnly = ( bbo_disabled ? false : true );
            document.getElementById("edit_nome").readOnly = ( bbo_disabled ? false : true );

            document.getElementById("edit_sobrenome").readOnly = ( bbo_disabled ? false : true );
            document.getElementById("edit_data_nascimento").readOnly = ( bbo_disabled ? false : true );

            document.getElementById("btn_sus_card").readOnly = ( bbo_disabled ? false : true );
            document.getElementById("btn_insurance_card").readOnly = ( bbo_disabled ? false : true );

            document.getElementById("edit_sus_card").disabled = ( bbo_disabled ? false : true );
            document.getElementById("edit_insurance_card").disabled = ( bbo_disabled ? false : true );

            document.getElementById("btn_sus_card").disabled = ( bbo_disabled ? false : true );
            document.getElementById("btn_insurance_card").disabled = ( bbo_disabled ? false : true );

            document.getElementById("select_sexo").readOnly = ( bbo_disabled ? false : true );
            document.getElementById("select_bloodType").readOnly = ( bbo_disabled ? false : true );
            document.getElementById("select_transfusao_sanguinea").readOnly = ( bbo_disabled ? false : true );

            document.getElementById("edit_peso").readOnly = ( bbo_disabled ? false : true );
            document.getElementById("edit_altura").readOnly = ( bbo_disabled ? false : true );

            document.getElementById("select_sexo").disabled = ( bbo_disabled ? false : true );
            document.getElementById("select_bloodType").disabled = ( bbo_disabled ? false : true );
            document.getElementById("select_transfusao_sanguinea").disabled = ( bbo_disabled ? false : true );


            //document.getElementById("btn_save_form_people").className = ( bbo_disabled ? 'btn btn-warning' : 'btn btn-default' );
            document.getElementById('btn_edit_form_people').className = ( bbo_disabled ? 'btn btn-warning' : 'btn btn-default' );
            document.getElementById('btn_edit_form_people').innerHTML = ( bbo_disabled ? '<i id="icon_edit_form_user" class="fa fa-ban"></i>&nbsp;Cancelar': '<i id="icon_edit_form_user" class="fa fa-edit"></i>&nbsp;Editar')

          } catch(e){
            console.log(e)
          }


        }//(disabledForm()
        //********************************************************************
      , populaceForm( p_obj_user, p_obj_people, p_obj_peopleHealth ){
          // Obj.: form population based on obj_user. If not, release form
          //******************************************************************
          try {

            console.log( )
            if ( p_obj_people ) {
              console.log( "objeto_pessoa", p_obj_people )
              document.getElementById('edit_codigo_pessoa').value = p_obj_people.id_people;
              document.getElementById('edit_nome').value = p_obj_people.name;
              document.getElementById('edit_sobrenome').value = p_obj_people.surname;
              document.getElementById('edit_data_nascimento').value = p_obj_people.birthday;
              document.getElementById('select_sexo').value = p_obj_people.sex;

              if ( p_obj_peopleHealth ){
                document.getElementById('edit_sus_card').value = p_obj_people.id_people;
                document.getElementById('edit_insurance_card').value = p_obj_people.name;
                document.getElementById('select_bloodType').value = p_obj_people.surname;
                document.getElementById('select_transfusao_sanguinea').value = p_obj_people.birthday;
                document.getElementById('edit_peso').value = p_obj_people.birthday;
                document.getElementById('edit_altura').value = p_obj_people.height;
                document.getElementById('edit_peso').value = p_obj_people.weight;
                //document.getElementById('select_sexo').value = p_obj_people.sex;
              } else {
                console.log(">>NÃO TEM HEALTH<<");
              }

            } else {
              if ( p_obj_user ){
                console.log( "objeto_usuario" )
                document.getElementById('edit_codigo_pessoa').valu = p_obj_user.id;
                document.getElementById('edit_nome').value = p_obj_user.name;
              }
            }



          } catch(e){
            cosole.log( "Erro ao popular form" )
          }

        }// fim -> populaForm()
      , f_resolve(){
          alert('Resolve');
        }
      , f_reject(){
          alert('Reject')
        }
        //**************************************************************
      , async postPessoaAjax(p_objeto_post) {
          //**************************************************************
          var sUrl = '/pessoa';
          var sType = "POST"
          console.log('POST AJAX')
          try {

            let edit_codigo_pessoa = document.getElementById('edit_codigo_pessoa').value
            let edit_nome = document.getElementById('edit_nome').value
            let edit_sobrenome = document.getElementById('edit_sobrenome').value
            let edit_data_nascimento = document.getElementById('edit_data_nascimento').value
            let select_sexo = document.getElementById('select_sexo').value;
            let edit_peso = document.getElementById('edit_peso').value;
            let edit_altura = document.getElementById('edit_altura').value;


            let select_bloodType = document.getElementById('select_bloodType').value
            let select_transfusao_sanguinea = document.getElementById('select_transfusao_sanguinea').value

            var obj_post = '{';

            console.log("Edit >>", edit_codigo_pessoa );
            if ( Number( edit_codigo_pessoa ) > 0 ){
              obj_post = obj_post + '"id_people": ' + Number( edit_codigo_pessoa ) + ",";
              sType = "POST";

            }

            if ( edit_nome.length > 2 ){
              obj_post = obj_post + '"name": "' + document.getElementById('edit_nome').value + '",';
            }

            if ( edit_sobrenome.length > 2 ){
              obj_post = obj_post + '"surname": "' + document.getElementById('edit_sobrenome').value + '",';
            }

            if ( document.getElementById('btn_sus_card').className === 'btn btn-success btn-flat' ) {
              obj_post = obj_post + '"sus_card": "' + document.getElementById('edit_sus_card').value + '",';
            }

            if ( document.getElementById('btn_insurance_card').className === 'btn btn-success btn-flat' ) {
              obj_post = obj_post + '"issurance_card": "' + document.getElementById('edit_insurance_card').value + '",';
            }

            obj_post = obj_post + '"birthday": "' + edit_data_nascimento + '",';

            obj_post = obj_post + '"sex": "' + select_sexo + '",';

            obj_post = obj_post + '"weight": ' + edit_peso + ',';

            obj_post = obj_post + '"height": ' + edit_altura + ',';

            obj_post = obj_post + '"blood_type": "' + select_bloodType + '",'

            obj_post = obj_post + '"blood_transfusion": "'+ select_transfusao_sanguinea +'"}';

            console.log('>> Vou montar objeto <<', obj_post )
            try {
              obj_post = JSON.parse(obj_post);
              console.log(">> Objeto Montado <<", obj_post );
            } catch( obj_error ){
              alert('Atenção!! Verifique as informações');
            }


          } catch( obj_error ){
            console.log( "Erro >> ",e," << ao montar objeto");
          }

          //return false;

          try {
            console.log('=> Vou postar')
            console.log( "URL: ", sUrl )
            console.log( "TYPE: ", sType )
            console.log( "OBJ: ", obj_post );

            await $.ajax({
              url         : sUrl,
              type       	: sType,
              data 				: obj_post,
              dataType    : 'JSON',
              headers: { 'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')},
              success: function(obj_retorno){
                console.log( obj_retorno.id_people )
                document.getElementById('edit_codigo_pessoa').value = obj_retorno.id_people;
                obj_retorno.people_health = JSON.parse( obj_retorno.people_health );
              },
              error: function(obj_erro){
                console.log(">> ERRO AO EXECUTAR POST <<");
                console.log( obj_erro )

              },
            })// $.ajax()
          } catch(e){
            console.log(e)
            alert("Erro ao Postar");
          }

        }

        //********************************************************************
      , postUser(){
          //******************************************************************
          let sUrl = '/admin/perfil';

          let obj_form_control = document.getElementsByClassName('form-control')
          console.log( obj_form_control )

        }// fim -> getUser()
      , validBirthday(){
        //******************************************************************
          let dt_date = Number(document.getElementById('edit_data_nascimento').value.substring(0,4));
          let dt_now =  Number(new Date().getFullYear());
          console.log( dt_now - dt_date );
          let soma = dt_now - dt_date;
          if ( soma < 18 ){
            alert( 'ATENÇÃO! FAVOR VERIFICAR A NECESSIDADE DE CADASTRAR UM RESPONSÁVEL!!');
          }
        }//validaBirthday
        //********************************************************************
      , f_change(){
        //********************************************************************
          //
          console.log( "f_change()" );
          let edit_name = document.getElementById('edit_nome').value;
          let select_sexo = document.getElementById('select_sexo').value;
          let select_bloodType = document.getElementById('select_bloodType').value;

          let bbo_valido = true;

          if ( edit_name.length < 3 ){
            console.log( "nome é menor" );
            bbo_valido = false;
          }

          console.log( select_sexo.value );
          if ( select_sexo.value < 1 ){
            console.log( "sexo tiu" );
            bbo_valido = false;
          }

          if ( select_bloodType < 1 ){
            console.log('BloodType')
            bbo_valido = false
          }

          console.log( bbo_valido )

          document.getElementById('btn_save_form_people').disabled = (bbo_valido ? false : true)
        }
    },
    mounted() {
      console.log( this )
      console.log("caralho>>", this.peopleHealth )
      const OBJ_THIS = this;
      this.execute( this );
      const f_change = this.f_change();

      $('#select_bloodType').on('click', function(){
        f_change
      })
    },
}
</script>


<template>
  <div>
    <div class="box box-success">
      <div class="box-header">
        <h3 class="box-title" name="label_cadastro"><i class="fa fa-user"></i>&nbsp;Dados Pessoais</h3>
        <div class="box-tools pull-right">
          <button type="button" class="btn btn-box-tool" data-widget="collapse"><i class="fa fa-minus"></i>
          </button>
        </div>
      </div>

      <div class="box-body" vfor="user in info">
          <form id="form_pessoa">

            <div class="form-group">
              <label for="edit_codigo_pessoa">Código da Pessoa</label>
              <input ref="edit_codigo_pessoa" class="form-control" type="text" name="edit_codigo_pessoa" id="edit_codigo_pessoa" value="" placeholder="Código" readonly>
            </div>

            <div class="form-group">
              <label for="edit_nome">Nome do Usuário</label>
              <input ref="edit_nome" class="form-control text-uppercase" type="text" name="edit_nome" id="edit_nome" value="" placeholder="Nome do usuário" readonly @change="f_change(this)">
            </div>

            <div class="form-group">
              <label for="edit_nome">Sobrenome</label>
              <input ref="edit_sobrenome" class="form-control text-uppercase" type="text" name="edit_sobrenome" id="edit_sobrenome" value="" placeholder="Sobrenome" readonly @change="f_change(this)">
            </div>


            <div class="form-group">
              <label for="edit_dtMovimentacao">Data de Nascimento</label>
              <input ref="edit_data_nascimento" class="form-control" type="date" name="edit_data_nascimento" id="edit_data_nascimento" @blur="validBirthday()"  readonly @change="f_change()">
            </div>

            <div class="form-group">
              <label>Sexo</label>
              <select class="form-control" name="select_sexo" id="select_sexo" readonly="true" @click="f_change()">
                <option value="M">Masculino</option>
                <option value="F">Feminino</option>
              </select>
            </div>

            <div class="row">
              <div class="col-md-6">
                <div class="form-group">
                  <label>Peso</label>
                  <input ref="edit_peso" class="form-control" type="number" min="0"  name="edit_peso" id="edit_peso" readonly="true">
                </div>
              </div>
              <div class="col-md-6">
                <label>Altura</label>
                <input ref="edit_altura" class="form-control" type="number" min="0.00"  name="edit_altura" id="edit_altura" readonly="true">
              </div>
            </div>

            <div class="row">
              <div class="col-md-6">
                <vc-input-sus-card  @change="f_change()"></vc-input-sus-card>
              </div>
              <div class="col-md-6">
                <vc-input-insurance-card  @change="f_change()"></vc-input-insurance-card>
              </div>
            </div>


          <div class="row">
            <div class="col-md-6">
              <vc-select-blood-type readonly="true"  @change="f_change()"></vc-select-blood-type>
            </div>
            <div class="col-md-6">
              <div class="form-group">
                <label>Transfusão sanguinea?</label>
                <select class="form-control" id="select_transfusao_sanguinea" name="select_transfusao_sanguinea" readonly @click="f_change()">
                  <option value="1">Aceito</option>
                  <option value="0">Não aceito</option>
                </select>
              </div>
            </div>

          </div>

        </form>
      </div><!-- box-body -->

      <div class="box-footer">
        <button type="button" id="btn_edit_form_people" @click="disabledForm()" class="btn btn-default"><i class="fa fa-edit"></i>&nbsp;Editar</button>
        <button type="button" id="btn_save_form_people" @click="postPessoaAjax()" class="btn btn-success pull-right" disabled="true">Salvar&nbsp;<i class="fa fa-save"></i></button>
      </div><!-- box-footer -->
    </div><!-- box box-primary -->
  </div>
</template>

<styles>

</styles>
