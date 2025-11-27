# ServiceNow Password Reset
Este repositório conterá documentação desenvolvida em meu PDI do ServiceNow, que mostra os ajustes nas ACLs, UI Actions e Configurações do Sistema para permitir que uma função execute redefinições de senha, além do usuário administrador.

________________________________________
Runbook – Correção Completa do Modal Set Password no ServiceNow
# 1. Objetivo
	Este runbook documenta todas as alterações necessárias para permitir que um usuário não-admin consiga:
	
		•	Gerar senha
		•	Visualizar a senha gerada
		•	Copiar a senha
		•	Utilizar o modal Set Password corretamente
________________________________________
# 2. Propriedades do Sistema
 	2.1 Habilitar o campo de senha
		•	Nome: glide.password.policy.generate.password.field.disabled
		•	Valor correto: false
		
	2.2 Outras propriedades recomendadas
		•	glide.user.show.password.field = true
		•	glide.security.password.reset.hide = false
________________________________________
# 3. Script Include – PasswordPolicyUtil
# O Script Include PasswordPolicyUtil é utilizado pelo modal para gerar e retornar a senha ao frontend.
	Foi identificado que a ACL associada a ele bloqueava a execução para usuários sem a role admin.
	Correção aplicada:
		Adicionar a role desejada (ex.: password_reset_admin) na ACL:
		•	Tipo: client_callable_script_include
		•	Nome: ACL associada ao PasswordPolicyUtil
________________________________________
# 4. ACLs da tabela sys_user
# Para permitir leitura, escrita e funcionamento dos botões no modal, foram ajustadas as seguintes ACLs:
	4.1 ACLs do campo user_password na tabela sys_user
		•	sys_user.user_password — operation = read
		•	sys_user.user_password — operation = write
		•	sys_user.user_password — operation = query_range
		
	4.2 ACL adicional utilizada pelo modal
		•	generate_copy_password — operation = read
Em todas essas ACLs, a única ação necessária foi adicionar a role responsável pelo reset de senha.
________________________________________
# 5. UI Actions – Botões do Modal Set Password
# Foi identificado que as UI Actions tinham condições que restringiam o uso apenas para a role admin.
	Correção aplicada nos botões abaixo: 
		•	Campo condition - (user.hasRole('admin') || user.hasRole('password_reset_admin'))
			•	Botão Generate Password
			•	Botão Reset Password
			•	Botão Set Password
________________________________________

