# Projeto: Análise Geográfica de Pontos de Ônibus em São Paulo

import random
import folium
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

sns.set(style="whitegrid")

# -----------------------------
# Etapa 1: Gerar dados fictícios com função
# -----------------------------
def gerar_dados_onibus(qtd_pontos=100):
    LAT_RANGE = (-23.7, -23.4)
    LON_RANGE = (-46.8, -46.4)
    zonas = ["Norte", "Sul", "Leste", "Oeste", "Centro"]
    movimentacao = ["Baixo", "Médio", "Alto"]
    classe_social = ["Alta", "Média", "Baixa"]
    ruas_exemplo = [f"Rua {i}" for i in range(1, 201)]

    dados = []

    for i in range(qtd_pontos):
        ponto = {
            "id": i + 1,
            "latitude": round(random.uniform(*LAT_RANGE), 6),
            "longitude": round(random.uniform(*LON_RANGE), 6),
            "zona": random.choice(zonas),
            "rua": random.choice(ruas_exemplo),
            "movimentacao": random.choices(movimentacao, weights=[0.2, 0.5, 0.3])[0],
            "classe_social": random.choices(classe_social, weights=[0.1, 0.5, 0.4])[0]
        }
        dados.append(ponto)

    return pd.DataFrame(dados)

# Gerar DataFrame
df_onibus = gerar_dados_onibus(100)

# -----------------------------
# Etapa 2: Criar mapa com cores por movimentação
# -----------------------------
def cor_por_movimentacao(movimento):
    if movimento == "Baixo":
        return "green"
    elif movimento == "Médio":
        return "orange"
    else:
        return "red"

def criar_mapa(df):
    mapa = folium.Map(location=[-23.55, -46.63], zoom_start=11)

    for _, ponto in df.iterrows():
        popup = (f"<b>Rua:</b> {ponto['rua']}<br>"
                 f"<b>Zona:</b> {ponto['zona']}<br>"
                 f"<b>Movimentação:</b> {ponto['movimentacao']}<br>"
                 f"<b>Classe:</b> {ponto['classe_social']}")

        folium.CircleMarker(
            location=[ponto["latitude"], ponto["longitude"]],
            radius=5,
            color=cor_por_movimentacao(ponto["movimentacao"]),
            fill=True,
            fill_opacity=0.7,
            fill_color=cor_por_movimentacao(ponto["movimentacao"]),
            popup=folium.Popup(popup, max_width=250)
        ).add_to(mapa)

    return mapa

# Criar e exibir mapa
mapa_onibus = criar_mapa(df_onibus)
mapa_onibus

# -----------------------------
# Etapa 3: Gráficos com seaborn
# -----------------------------
# Gráfico: Pontos por zona
plt.figure(figsize=(8, 4))
sns.countplot(data=df_onibus, x="zona", palette="Blues_d", order=sorted(df_onibus["zona"].unique()))
plt.title("Quantidade de Pontos por Zona")
plt.xlabel("Zona")
plt.ylabel("Quantidade")
plt.show()

# Gráfico: Movimentação por zona
plt.figure(figsize=(10, 5))
sns.countplot(data=df_onibus, x="zona", hue="movimentacao", palette="Set2", order=sorted(df_onibus["zona"].unique()))
plt.title("Distribuição da Movimentação por Zona")
plt.xlabel("Zona")
plt.ylabel("Quantidade")
plt.legend(title="Movimentação")
plt.show()

# Gráfico: Classe social por zona
plt.figure(figsize=(10, 5))
sns.countplot(data=df_onibus, x="zona", hue="classe_social", palette="Set1", order=sorted(df_onibus["zona"].unique()))
plt.title("Distribuição da Classe Social por Zona")
plt.xlabel("Zona")
plt.ylabel("Quantidade")
plt.legend(title="Classe Social")
plt.show()

# -----------------------------
# Etapa 4: Tabelas resumo com percentuais
# -----------------------------
# Tabela: Total de pontos por zona
df_zona = df_onibus.groupby("zona").size().reset_index(name="total_pontos")
df_zona["% do total"] = (df_zona["total_pontos"] / df_zona["total_pontos"].sum() * 100).round(2)
print("\nTotal de pontos por zona:")
print(df_zona)

# Tabela: Movimentação por zona
df_mov = df_onibus.groupby(["zona", "movimentacao"]).size().reset_index(name="qtd")
df_mov["% dentro da zona"] = df_mov.groupby("zona")["qtd"].transform(lambda x: round(100 * x / x.sum(), 2))
print("\nMovimentação por zona:")
print(df_mov)

# Tabela: Classe social por zona
df_classe = df_onibus.groupby(["zona", "classe_social"]).size().reset_index(name="qtd")
df_classe["% dentro da zona"] = df_classe.groupby("zona")["qtd"].transform(lambda x: round(100 * x / x.sum(), 2))
print("\nClasse social por zona:")
print(df_classe)
