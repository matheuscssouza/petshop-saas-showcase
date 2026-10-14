# Modelo de dados (resumo)

```
Tenant └─ Unidade (petshop/vet, CNPJ) ├─ Profissional └─ Sala
Tutor 1─N Pet 1─N Agendamento 1─1 Atendimento 1─N ItemAtendimento
Atendimento 1─1 Cobrança 1─N Pagamento · Split · NotaFiscal
Consulta 1─1 Prontuário 1─N Prescrição/Exame/Anexo
Pet 1─N Vacina · Tutor 1─N Assinatura
```

## Estados
- Agendamento: `agendado → confirmado → em_atendimento → concluido` (`cancelado`, `falta`, `remarcado`)
- Atendimento (banho/tosa): `aguardando → em_andamento → secagem → pronto → entregue`
- Cobrança: `pendente → paga → (parcialmente_estornada | estornada)` (`expirada`, `falhou`)

## Decisões
Valores em centavos · bruto/taxa/líquido por cobrança · preço congelado no item · nunca apagar dado financeiro/clínico.
