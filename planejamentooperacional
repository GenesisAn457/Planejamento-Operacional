import streamlit as st

st.set_page_config(page_title="Sistema", layout="wide")

# ✅ CSS GLOBAL (BOTÕES AZUIS)
st.markdown("""
    <style>
    div.stButton > button {
        background-color: #0077C8;
        color: white;
        border-radius: 6px;
        border: none;
        padding: 10px;
        font-weight: bold;
    }

    div.stButton > button:hover {
        background-color: #005fa3;
        color: white;
    }
    </style>
""", unsafe_allow_html=True)


# =========================
# ✅ SESSION
# =========================
if "logado" not in st.session_state:
    st.session_state.logado = False

if "usuario_logado" not in st.session_state:
    st.session_state.usuario_logado = ""

if "perfil" not in st.session_state:
    st.session_state.perfil = ""

if "usuarios" not in st.session_state:
    st.session_state.usuarios = {
        "admin": {
            "senha": "admin123",
            "perfil": "master"
        }
    }

if "tela_ativa" not in st.session_state:
    st.session_state.tela_ativa = "RESUMO"


# =========================
# ✅ FUNÇÕES
# =========================
def login(usuario, senha):
    usuarios = st.session_state.usuarios
    if usuario in usuarios and usuarios[usuario]["senha"] == senha:
        st.session_state.logado = True
        st.session_state.usuario_logado = usuario
        st.session_state.perfil = usuarios[usuario]["perfil"]
        return True
    return False


def cadastrar(usuario, senha, perfil):
    if usuario in st.session_state.usuarios:
        return False

    st.session_state.usuarios[usuario] = {
        "senha": senha,
        "perfil": perfil
    }
    return True


def atualizar_usuario(usuario, nova_senha, novo_perfil):
    st.session_state.usuarios[usuario]["senha"] = nova_senha
    st.session_state.usuarios[usuario]["perfil"] = novo_perfil


# =========================
# ✅ LOGIN
# =========================
if not st.session_state.logado:

    st.title("🔐 Acesso ao Sistema")
    st.markdown("### 🌲 Sistema de Planejamento Florestal")

    usuario = st.text_input("Usuário")
    senha = st.text_input("Senha", type="password")

    if st.button("Entrar"):
        if login(usuario, senha):
            st.success("✅ Login realizado!")
            st.rerun()
        else:
            st.error("❌ Usuário ou senha inválidos")

# =========================
# ✅ SISTEMA
# =========================
else:

    usuario_logado = st.session_state.usuario_logado
    perfil = st.session_state.perfil

    # MENU LATERAL
    st.sidebar.title("📊 Menu")

    opcao = st.sidebar.selectbox(
        "Navegação",
        ["Dashboard", "Cadastro de Usuários"]
    )

    if st.sidebar.button("Sair"):
        st.session_state.logado = False
        st.session_state.usuario_logado = ""
        st.session_state.perfil = ""
        st.rerun()

    # =========================
    # ✅ FAIXA ANIMADA (MAIS LENTA)
    # =========================
    st.markdown("""
        <style>
        .marquee {
            width: 100%;
            overflow: hidden;
            white-space: nowrap;
            background-color: #0077C8;
            color: white;
            padding: 10px 0;
            font-weight: bold;
            border-radius: 3px;
        }

        .marquee span {
            display: inline-block;
            padding-left: 100%;
            animation: marquee 40s linear infinite;
        }

        @keyframes marquee {
            0% { transform: translateX(0); }
            100% { transform: translateX(-100%); }
        }
        </style>

        <div class="marquee">
            <span>🌱 Planejar o manejo florestal com responsabilidade garante recursos e preservação para o futuro. 🌲</span>
        </div>
    """, unsafe_allow_html=True)

    # HEADER
    st.title(f"Bem-vindo, {usuario_logado} 👋")
    st.write("### 🌲 Sistema de Planejamento Florestal")
    st.write(f"Perfil: **{perfil.upper()}**")

    st.divider()

    # =========================
    # ✅ DASHBOARD
    # =========================
    if opcao == "Dashboard":

        st.subheader("📊 Dashboard")

        telas = [
            "RESUMO", "GERAL", "MENSAL", "PRODUÇÃO", "PRODUÇÃO FORECAST",
            "PRODUÇÃO TORA", "TRANSFERÊNCIA", "TRANSFERÊNCIA FORECAST",
            "CARREGAMENTO", "COMPRA", "COMPRA FORECAST", "VALIDAÇÃO GUIA",
            "VENDA", "VENDA FORECAST", "VENDA POR CLIENTE",
            "CARG MAQ", "PRODUÇÃO MAQ", "EXTRATOR", "INVENTÁRIO"
        ]

        cols = st.columns(6)

        for i, tela in enumerate(telas):
            col = cols[i % 6]

            if col.button(tela):
                st.session_state.tela_ativa = tela
                st.rerun()

        st.divider()
        st.subheader(st.session_state.tela_ativa)

    # =========================
    # ✅ CADASTRO + LISTA
    # =========================
    elif opcao == "Cadastro de Usuários":

        if perfil == "master":

            st.subheader("👤 Cadastrar novo usuário")

            with st.form("cadastro"):
                novo_usuario = st.text_input("Usuário")
                nova_senha = st.text_input("Senha", type="password")
                tipo = st.selectbox("Perfil", ["user", "master"])

                if st.form_submit_button("Cadastrar"):
                    if cadastrar(novo_usuario, nova_senha, tipo):
                        st.success("Usuário cadastrado!")
                    else:
                        st.error("Usuário já existe")

            st.divider()

            # ✅ LISTA DE USUÁRIOS
            st.subheader("📋 Usuários cadastrados")

            for user, dados in st.session_state.usuarios.items():

                with st.expander(f"👤 {user}"):
                    nova_senha = st.text_input(
                        f"Nova senha ({user})",
                        value=dados["senha"],
                        key=f"senha_{user}"
                    )

                    novo_perfil = st.selectbox(
                        f"Perfil ({user})",
                        ["user", "master"],
                        index=0 if dados["perfil"] == "user" else 1,
                        key=f"perfil_{user}"
                    )

                    if st.button(f"Salvar {user}", key=f"btn_{user}"):
                        atualizar_usuario(user, nova_senha, novo_perfil)
                        st.success(f"{user} atualizado!")

        else:
            st.warning("Sem permissão")
