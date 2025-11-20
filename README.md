# Psc
Cliente {
	idCliente integer pk increments unique
	nome varchar
	telefone integer
	endereço varchar
	CPF integer
	e-mail varchar
}

Funcionário {
	idFuncionario integer pk increments unique
	nome varchar
	telefone integer
	endereço varchar
	CPF integer
	e-mail varchar
	currículo integer
}

Procedimentos {
	idProcedimentos integer pk increments unique
	nome varchar
	preço integer
	descrição varchar
}

Avaliações {
	idAvaliações integer pk increments unique
	idProcedimento integer > Procedimentos.idProcedimentos
	nota integer
	data integer
	idCliente integer > Cliente.idCliente
	comentário varchar
}

Agendamentos {
	idAgendamentos integer pk increments unique
	idProcedimentos integer > Procedimentos.idProcedimentos
	idFuncionário integer > Funcionário.idFuncionario
	idCliente integer > Cliente.idCliente
	data integer
	horário integer
	local varchar
	metodo_de_pagamento varchar
}

Vendas {
	idVendas integer pk increments unique
	idProcedimentos integer > Procedimentos.idProcedimentos
	data integer
}

Vendas_detalhe {
	idVendasDetalhe integer pk increments unique
	idProcedimentos integer > Procedimentos.idProcedimentos
	idPagamento integer > Agendamentos.metodo_de_pagamento
	idCliente integer > Cliente.idCliente
	idVendas integer > Vendas.idVendas
}
