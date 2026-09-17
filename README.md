# Sistema de Marcenaria - Controle de Madeira

print("=== SISTEMA DE MARCENARIA ===")

nome_movel = input("Nome do móvel: ")
tipo_madeira = input("Tipo de madeira: ")

comprimento = float(input("Comprimento da peça (cm): "))
largura = float(input("Largura da peça (cm): "))
espessura = float(input("Espessura da peça (cm): "))
quantidade = int(input("Quantidade de peças: "))

# Cálculo do volume de uma peça em cm³
volume_peca = comprimento * largura * espessura

# Volume total de madeira
volume_total = volume_peca * quantidade

print("\n=== ORÇAMENTO DA MADEIRA ===")
print(f"Móvel: {nome_movel}")
print(f"Madeira: {tipo_madeira}")
print(f"Quantidade de peças: {quantidade}")
print(f"Volume por peça: {volume_peca:.2f} cm³")
print(f"Volume total: {volume_total:.2f} cm³")
print(f"Volume total: {volume_total / 1000000:.4f} m³")
