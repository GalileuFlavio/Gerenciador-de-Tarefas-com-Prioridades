# Gerenciador-de-Tarefas-com-Prioridades
Preenchimento: use os comandos no menu interativo


"""
Gerenciador de Tarefas com Prioridades
Preenchimento: use os comandos no menu interativo
"""
import json
from datetime import datetime
from pathlib import Path

class TodoList:
    def __init__(self):
        self.arquivo = "tarefas.json"
        self.tarefas = self.carregar()
    
    def carregar(self):
        if Path(self.arquivo).exists():
            with open(self.arquivo, 'r', encoding='utf-8') as f:
                return json.load(f)
        return []
    
    def salvar(self):
        with open(self.arquivo, 'w', encoding='utf-8') as f:
            json.dump(self.tarefas, f, indent=2, ensure_ascii=False)
    
    def adicionar(self, titulo, prioridade="média"):
        """Adiciona nova tarefa - preencha titulo e prioridade"""
        tarefa = {
            "id": len(self.tarefas) + 1,
            "titulo": titulo,           # ← PREENCHA: descrição da tarefa
            "prioridade": prioridade,   # ← PREENCHA: baixa, média ou alta
            "concluida": False,
            "criada": datetime.now().strftime("%Y-%m-%d %H:%M")
        }
        self.tarefas.append(tarefa)
        self.salvar()
        print(f"✅ Tarefa '{titulo}' adicionada!")
    
    def listar(self):
        """Mostra todas as tarefas"""
        if not self.tarefas:
            print("📭 Nenhuma tarefa encontrada.")
            return
        
        print("\n📋 Minhas Tarefas:")
        print("-" * 50)
        for t in self.tarefas:
            status = "✅" if t["concluida"] else "⬜"
            print(f"{status} [{t['prioridade'].upper()}] {t['titulo']}")
            print(f"   Criada em: {t['criada']}")
            print("-" * 50)
    
    def concluir(self, titulo):
        """Marca tarefa como concluída"""
        for t in self.tarefas:
            if t["titulo"].lower() == titulo.lower():
                t["concluida"] = True
                self.salvar()
                print(f"✅ Tarefa '{titulo}' concluída!")
                return
        print("❌ Tarefa não encontrada.")

# === COMO USAR ===
# Execute e use o menu interativo:
# 1 = Adicionar (digite título e prioridade)
# 2 = Listar (veja todas)
# 3 = Concluir (digite o título exato)
# 4 = Sair

if __name__ == "__main__":
    app = TodoList()
    
    while True:
        print("\n📝 GERENCIADOR DE TAREFAS")
        print("1. Adicionar tarefa")
        print("2. Listar tarefas")
        print("3. Concluir tarefa")
        print("4. Sair")
        
        opcao = input("\nEscolha: ")
        
        if opcao == "1":
            titulo = input("Título: ")
            prioridade = input("Prioridade (baixa/média/alta): ") or "média"
            app.adicionar(titulo, prioridade)
        elif opcao == "2":
            app.listar()
        elif opcao == "3":
            titulo = input("Título da tarefa a concluir: ")
            app.concluir(titulo)
        elif opcao == "4":
            break
