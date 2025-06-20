<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Análise Interativa: Conexões entre Educação Crítica e IA</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        /* Chosen Palette: Calm Harmony */
        /* Application Structure Plan: A single-page application (SPA) with a top navigation bar for thematic sections (Intro, Theories, Practice, Challenges, Conclusion). This structure allows users to either follow a linear path or jump to sections of interest. The design uses interactive cards, diagrams, and a dynamic chart to make dense academic concepts (like Rabardel's Instrumental Genesis and Skovsmose's Critical Education) more digestible and engaging. The goal is to transform a static paper into an exploratory learning experience. */
        /* Visualization & Content Choices: 
           - Report Info: Core theories (Skovsmose, Rabardel, Duval). Goal: Inform/Organize. Viz/Presentation: Interactive cards and diagrams built with HTML/CSS. Interaction: Click/hover to reveal details. Justification: Simplifies complex theories. Library/Method: Vanilla JS, Tailwind.
           - Report Info: Teenage pregnancy data. Goal: Compare/Inform. Viz/Presentation: Interactive bar chart. Interaction: Hover for tooltips, contextual text alongside. Justification: Brings static data to life. Library/Method: Chart.js.
           - Report Info: AI-generated problems. Goal: Compare/Analyze. Viz/Presentation: Side-by-side comparison in a modal. Interaction: Button to trigger modal. Justification: Direct comparison with added analytical layer. Library/Method: Vanilla JS, Tailwind.
           - Report Info: SDAI Dimensions. Goal: Organize/Inform. Viz/Presentation: Clickable tabs. Interaction: Click to switch content. Justification: Organizes related concepts efficiently. Library/Method: Vanilla JS, Tailwind.
        */
        /* CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc; /* slate-50 */
            color: #1e293b; /* slate-800 */
        }
        .section-title {
            color: #1e3a8a; /* blue-800 */
        }
        .card {
            background-color: white;
            border-radius: 0.75rem;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
        }
        .btn-primary {
            background-color: #1d4ed8; /* blue-700 */
            color: white;
            transition: background-color 0.3s;
        }
        .btn-primary:hover {
            background-color: #1e40af; /* blue-800 */
        }
        .btn-secondary {
            background-color: #f1f5f9; /* slate-100 */
            color: #334155; /* slate-700 */
            transition: background-color 0.3s;
        }
        .btn-secondary:hover {
            background-color: #e2e8f0; /* slate-200 */
        }
        .nav-link {
            transition: color 0.3s, border-bottom-color 0.3s;
            border-bottom: 2px solid transparent;
        }
        .nav-link:hover, .nav-link.active {
            color: #1d4ed8;
            border-bottom-color: #1d4ed8;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 400px;
            }
        }
        .interactive-diagram-node {
            cursor: pointer;
            transition: all 0.2s ease-in-out;
        }
        .interactive-diagram-node:hover {
            transform: scale(1.05);
            background-color: #dbeafe; /* blue-100 */
        }
        .hidden-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.5s ease-in-out;
        }
        .visible-content {
            max-height: 500px; /* Adjust as needed */
        }
        .tabs button.active {
            border-color: #1d4ed8;
            color: #1d4ed8;
            background-color: #eff6ff; /* blue-50 */
        }
    </style>
</head>
<body class="antialiased">

    <!-- Header & Navigation -->
    <header class="bg-white/80 backdrop-blur-md shadow-sm sticky top-0 z-50">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <div class="flex-shrink-0">
                    <h1 class="text-xl font-bold text-slate-800">IA & Educação Crítica</h1>
                </div>
                <div class="hidden md:block">
                    <div class="ml-10 flex items-baseline space-x-4">
                        <a href="#intro" class="nav-link px-3 py-2 rounded-md text-sm font-medium">Introdução</a>
                        <a href="#teoria" class="nav-link px-3 py-2 rounded-md text-sm font-medium">Fundamentos Teóricos</a>
                        <a href="#pratica" class="nav-link px-3 py-2 rounded-md text-sm font-medium">IA na Prática</a>
                        <a href="#desafios" class="nav-link px-3 py-2 rounded-md text-sm font-medium">Desafios e Riscos</a>
                        <a href="#conclusao" class="nav-link px-3 py-2 rounded-md text-sm font-medium">Conclusão</a>
                    </div>
                </div>
            </div>
        </nav>
    </header>

    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-8">

        <!-- Seção de Introdução -->
        <section id="intro" class="text-center py-12 md:py-20">
            <h2 class="text-3xl md:text-5xl font-extrabold text-slate-900 tracking-tight leading-tight">
                Conexões entre a Educação Crítica e o Uso da Inteligência Artificial
            </h2>
            <p class="mt-4 text-lg md:text-xl text-slate-600 max-w-3xl mx-auto">
                Uma análise interativa sobre o desenvolvimento do letramento estatístico nos anos iniciais, explorando como a IA pode revolucionar o ensino, mas apenas com uma abordagem crítica e reflexiva.
            </p>
            <div class="mt-8">
                <a href="#teoria" class="btn-primary font-semibold py-3 px-6 rounded-lg shadow-lg">Comece a Explorar</a>
            </div>
        </section>

        <!-- Seção Fundamentos Teóricos -->
        <section id="teoria" class="py-16">
            <h3 class="text-3xl font-bold text-center section-title mb-12">Fundamentos Teóricos Essenciais</h3>
            <div class="grid md:grid-cols-1 lg:grid-cols-3 gap-8">
                <!-- Card: Educação Crítica -->
                <div class="card p-6">
                    <h4 class="text-xl font-bold text-slate-800 mb-3">Educação Matemática Crítica</h4>
                    <p class="text-slate-600 mb-4">Proposta por Ole Skovsmose, esta abordagem vê a matemática como uma ferramenta para ler, questionar e intervir no mundo. A estatística, em particular, é usada para analisar problemas sociais e desenvolver a "matemacia" – a capacidade de usar a matemática de forma crítica.</p>
                    <div class="bg-blue-50 border-l-4 border-blue-500 text-blue-800 p-4 rounded-r-lg">
                        <p class="font-semibold">Citação Chave:</p>
                        <blockquote class="mt-2 italic">"A educação deve preparar os estudantes não só para 'fazer contas', mas para 'ler o mundo com olhar questionador'."</blockquote>
                    </div>
                </div>
                <!-- Card: Gênese Instrumental -->
                <div class="card p-6">
                    <h4 class="text-xl font-bold text-slate-800 mb-3">Teoria da Gênese Instrumental</h4>
                    <p class="text-slate-600 mb-4">Desenvolvida por Pierre Rabardel, explica como um **artefato** (a IA) se torna um **instrumento** de aprendizagem. Clique nos conceitos abaixo para entender o processo.</p>
                    <div id="rabardel-diagram" class="space-y-3">
                        <div class="interactive-diagram-node bg-slate-100 p-3 rounded-lg" onclick="toggleContent('instrumentacao-content')">
                            <h5 class="font-bold">Instrumentação</h5>
                            <p id="instrumentacao-content" class="hidden-content text-slate-600 mt-2 text-sm">Ocorre quando o professor se apropria das funcionalidades básicas da IA, aprendendo a operá-la. É a adaptação do usuário à ferramenta.</p>
                        </div>
                        <div class="text-center text-slate-500 text-2xl font-bold">↓ ↑</div>
                        <div class="interactive-diagram-node bg-slate-100 p-3 rounded-lg" onclick="toggleContent('instrumentalizacao-content')">
                            <h5 class="font-bold">Instrumentalização</h5>
                            <p id="instrumentalizacao-content" class="hidden-content text-slate-600 mt-2 text-sm">Ocorre quando o professor ressignifica a ferramenta, adaptando-a criativamente para seus objetivos pedagógicos, como criar planos de aula ou analisar dados de forma crítica. É a adaptação da ferramenta pelo usuário.</p>
                        </div>
                    </div>
                </div>
                <!-- Card: Representações Semióticas -->
                <div class="card p-6">
                    <h4 class="text-xl font-bold text-slate-800 mb-3">Representações Semióticas</h4>
                    <p class="text-slate-600 mb-4">Segundo Raymond Duval, a compreensão matemática exige a capacidade de transitar entre diferentes formas de representar um mesmo conceito. O desafio não está em operar dentro de um sistema, mas em converter entre eles.</p>
                    <div id="duval-diagram" class="space-y-3">
                        <div class="interactive-diagram-node bg-slate-100 p-3 rounded-lg" onclick="toggleContent('tratamento-content')">
                            <h5 class="font-bold">Tratamento</h5>
                            <p id="tratamento-content" class="hidden-content text-slate-600 mt-2 text-sm">Operar dentro do mesmo sistema de representação. Exemplo: simplificar a fração 2/4 para 1/2.</p>
                        </div>
                        <div class="text-center text-slate-500 text-2xl font-bold">↔</div>
                        <div class="interactive-diagram-node bg-slate-100 p-3 rounded-lg" onclick="toggleContent('conversao-content')">
                            <h5 class="font-bold">Conversão</h5>
                            <p id="conversao-content" class="hidden-content text-slate-600 mt-2 text-sm">Mudar de um sistema para outro. Exemplo: transformar a fração 1/2 em um gráfico de pizza ou no número decimal 0,5. É aqui que reside a maior dificuldade cognitiva.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Seção IA na Prática -->
        <section id="pratica" class="py-16 bg-white">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <h3 class="text-3xl font-bold text-center section-title mb-12">A IA na Prática: Estudos de Caso e Ferramentas</h3>
                
                <div class="tabs flex justify-center space-x-2 sm:space-x-4 border-b mb-8">
                    <button class="tab-btn active" onclick="openTab(event, 'tab-caso-1')">Gravidez na Adolescência</button>
                    <button class="tab-btn" onclick="openTab(event, 'tab-caso-2')">Geração de Problemas</button>
                    <button class="tab-btn" onclick="openTab(event, 'tab-caso-3')">Dimensões da SDAI</button>
                </div>

                <!-- Tab 1: Estudo de Caso - Gravidez na Adolescência -->
                <div id="tab-caso-1" class="tab-content">
                    <h4 class="text-2xl font-semibold text-center mb-8 text-slate-700">Estudo de Caso: Análise de Dados sobre Gravidez na Adolescência</h4>
                    <p class="text-center text-slate-600 max-w-3xl mx-auto mb-10">Um projeto com alunos do 5º ano utilizou dados reais do IBGE e DATASUS para desenvolver o letramento estatístico. A IA foi usada como ferramenta de apoio para pesquisa e análise, permitindo uma discussão crítica sobre um problema social relevante.</p>
                    <div class="grid lg:grid-cols-2 gap-8 items-center">
                        <div class="card p-6">
                            <h5 class="font-bold text-lg mb-4 text-center">Nascidos Vivos de Mães Adolescentes (2018)</h5>
                            <div class="chart-container">
                                <canvas id="gravidezChart"></canvas>
                            </div>
                            <figcaption class="text-xs text-center mt-2 text-slate-500">Fonte: IBGE (2018)</figcaption>
                        </div>
                        <div class="card p-6">
                            <h5 class="font-bold text-lg mb-4 text-center">Índices por Faixa Etária (2019)</h5>
                            <table class="w-full text-left text-sm">
                                <thead class="bg-slate-100">
                                    <tr>
                                        <th class="p-3 font-semibold">Região</th>
                                        <th class="p-3 font-semibold text-center">10 a 14 anos</th>
                                        <th class="p-3 font-semibold text-center">15 a 19 anos</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr class="border-b">
                                        <td class="p-3 font-medium">Norte</td>
                                        <td class="p-3 text-center">4,8%</td>
                                        <td class="p-3 text-center">79,9%</td>
                                    </tr>
                                    <tr class="border-b">
                                        <td class="p-3 font-medium">Nordeste</td>
                                        <td class="p-3 text-center">3,1%</td>
                                        <td class="p-3 text-center">54,5%</td>
                                    </tr>
                                    <tr class="border-b">
                                        <td class="p-3 font-medium">Sudeste</td>
                                        <td class="p-3 text-center">1,6%</td>
                                        <td class="p-3 text-center">38,2%</td>
                                    </tr>
                                     <tr class="border-b">
                                        <td class="p-3 font-medium">Sul</td>
                                        <td class="p-3 text-center">1,5%</td>
                                        <td class="p-3 text-center">38,9%</td>
                                    </tr>
                                    <tr>
                                        <td class="p-3 font-medium">Centro-Oeste</td>
                                        <td class="p-3 text-center">2,6%</td>
                                        <td class="p-3 text-center">50,1%</td>
                                    </tr>
                                </tbody>
                            </table>
                             <figcaption class="text-xs text-center mt-2 text-slate-500">Fonte: DATASUS/SINASC (2019)</figcaption>
                        </div>
                    </div>
                </div>

                <!-- Tab 2: Geração de Problemas -->
                <div id="tab-caso-2" class="tab-content hidden">
                    <h4 class="text-2xl font-semibold text-center mb-8 text-slate-700">Geração de Problemas com IA: Comparativo</h4>
                    <p class="text-center text-slate-600 max-w-3xl mx-auto mb-10">Foi solicitado a duas IAs (ChatGPT e DeepSeek) que criassem um problema de estatística para crianças de 9 anos, contextualizado na periferia de Franco da Rocha, SP. Veja a diferença nas abordagens.</p>
                     <div class="grid md:grid-cols-2 gap-8">
                        <div class="card p-6">
                            <h5 class="font-bold text-lg mb-2">Proposta do ChatGPT</h5>
                            <p class="text-sm text-slate-500 mb-4">Foco em mobilidade urbana</p>
                            <div class="prose prose-sm max-w-none">
                                <p><strong>Problema:</strong> Imagine que você mora em uma comunidade em Franco da Rocha e percebe que muitas pessoas têm dificuldades de locomoção. O transporte público demora a passar. Você decide fazer uma pesquisa com 10 vizinhos sobre como eles se locomovem e o que acham do transporte público.</p>
                                <p><strong>Análise Pedagógica:</strong> A proposta é aberta e relevante, mas depende de dados fictícios ou de uma coleta real. Foca na interpretação de dados e na reflexão sobre melhorias comunitárias.</p>
                            </div>
                        </div>
                        <div class="card p-6">
                            <h5 class="font-bold text-lg mb-2">Proposta da DeepSeek</h5>
                            <p class="text-sm text-slate-500 mb-4">Foco em infraestrutura escolar</p>
                            <div class="prose prose-sm max-w-none">
                                <p><strong>Problema:</strong> Lucas e seus amigos caminham até a escola e notam problemas como calçadas quebradas e falta de iluminação. Eles registram as ocorrências por uma semana para convencer a prefeitura a agir.</p>
                                <p><strong>Análise Pedagógica:</strong> Mais estruturada, já fornece uma tabela de dados para análise. Inclui desafios de cálculo (média, custo) e de ação social (criar cartazes), guiando mais o aluno.</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Tab 3: Dimensões da SDAI -->
                <div id="tab-caso-3" class="tab-content hidden">
                    <h4 class="text-2xl font-semibold text-center mb-8 text-slate-700">Dimensões dos Sistemas Didáticos Artificiais Inteligentes (SDAI)</h4>
                    <p class="text-center text-slate-600 max-w-3xl mx-auto mb-10">O estudo propõe um modelo de sistemas de IA para educação (SDAI) com quatro dimensões de personalização. Cada uma utiliza diferentes tipos de dados e possui riscos éticos específicos.</p>
                    <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-6">
                        <div class="card p-5 text-center">
                            <h5 class="font-bold text-lg mb-2">Perfilamento Educacional</h5>
                            <p class="text-sm text-slate-600"><strong>Foco:</strong> Desempenho acadêmico (notas, tempo de resposta).<br><strong>Risco:</strong> Reducionismo pedagógico.</p>
                        </div>
                        <div class="card p-5 text-center">
                            <h5 class="font-bold text-lg mb-2">Hiperpersonalização Biográfica</h5>
                            <p class="text-sm text-slate-600"><strong>Foco:</strong> Identidade e trajetória de vida (raça, gênero, saúde).<br><strong>Risco:</strong> Vieses discriminatórios.</p>
                        </div>
                        <div class="card p-5 text-center">
                            <h5 class="font-bold text-lg mb-2">Adaptação Sociodigital</h5>
                            <p class="text-sm text-slate-600"><strong>Foco:</strong> Contexto cultural e digital (redes sociais, localização).<br><strong>Risco:</strong> Manipulação por algoritmos.</p>
                        </div>
                        <div class="card p-5 text-center">
                            <h5 class="font-bold text-lg mb-2">Customização Comportamental</h5>
                            <p class="text-sm text-slate-600"><strong>Foco:</strong> Padrões de interação (cliques, erros, tempo de tela).<br><strong>Risco:</strong> Vigilância excessiva.</p>
                        </div>
                    </div>
                </div>

            </div>
        </section>

        <!-- Seção Desafios e Riscos -->
        <section id="desafios" class="py-16">
            <h3 class="text-3xl font-bold text-center section-title mb-12">Desafios, Riscos e Efeitos do Uso da IA</h3>
            <div class="grid lg:grid-cols-2 gap-8">
                <div class="card p-6">
                    <h4 class="text-xl font-bold text-slate-800 mb-3">Obstáculos à Instrumentalização</h4>
                    <p class="text-slate-600 mb-4">O uso não crítico da IA pode levar a efeitos negativos que impedem o desenvolvimento da autonomia intelectual do docente.</p>
                    <ul class="space-y-3">
                        <li class="p-3 bg-red-50 border-l-4 border-red-400 rounded-r-lg">
                            <strong class="text-red-800">Dependência Intelectual:</strong> O professor perde a capacidade de criar e questionar, tornando-se passivo.
                        </li>
                        <li class="p-3 bg-red-50 border-l-4 border-red-400 rounded-r-lg">
                            <strong class="text-red-800">Alienação Cognitiva:</strong> O docente defende as respostas da IA como se fossem suas, sem conexão com sua própria prática.
                        </li>
                         <li class="p-3 bg-amber-50 border-l-4 border-amber-400 rounded-r-lg">
                            <strong class="text-amber-800">Efeito Mentor Onipotente:</strong> A anulação do pensamento do docente, que se torna tutelado pela IA, perdendo a confiança para pensar e agir de forma autônoma.
                        </li>
                    </ul>
                </div>
                <div class="card p-6">
                    <h4 class="text-xl font-bold text-slate-800 mb-3">Interpretação de Detectores de IA</h4>
                    <p class="text-slate-600 mb-4">Detectores de IA são ferramentas indicativas, não conclusivas. Uma abordagem pedagógica é crucial para interpretar seus resultados e fornecer um feedback construtivo ao aluno.</p>
                     <div class="bg-gray-800 rounded-lg p-4 font-mono text-sm text-white">
                        <p class="text-green-400">> Relatório de Análise - Turnitin AI</p>
                        <p>> Texto analisado: [Ensaio sobre Educação Crítica]</p>
                        <p>> Resultado: <span class="bg-yellow-500 text-black px-2 py-1 rounded">40% de probabilidade de geração por IA.</span></p>
                        <br>
                        <p class="text-cyan-400">> Análise Pedagógica Sugerida:</p>
                        <p class="text-gray-300">"Isso não significa plágio, mas que o texto pode precisar de mais da sua voz autoral. Vamos revisar juntos os trechos destacados para fortalecer seus argumentos?"</p>
                     </div>
                     <p class="text-xs text-slate-500 mt-2">Esta é uma simulação para fins ilustrativos.</p>
                </div>
            </div>
        </section>

        <!-- Seção Conclusão -->
        <section id="conclusao" class="py-16 bg-white">
            <div class="max-w-4xl mx-auto text-center">
                <h3 class="text-3xl font-bold section-title mb-4">Conclusão: Rumo a um Futuro Crítico e Tecnológico</h3>
                <div class="prose prose-lg text-slate-700 mx-auto">
                    <p>A colaboração entre educação crítica e inteligência artificial é essencial para moldar um futuro educacional mais justo. A IA, instrumentalizada com criticidade, pode ser uma aliada poderosa na construção de uma sociedade mais justa. A verdadeira revolução não está na tecnologia em si, mas em como a mobilizamos para formar cidadãos que não apenas consomem dados, mas os interrogam, transformam e, sobretudo, os humanizam.</p>
                    <p>O futuro da educação exige um equilíbrio delicado: aproveitar o potencial da IA para personalizar o aprendizado, sem renunciar à formação de sujeitos autônomos. Isso demanda políticas de acesso equitativo, formação docente contínua e um currículo que integre pensamento computacional e humanístico.</p>
                </div>
            </div>
        </section>

    </main>

    <footer class="bg-slate-800 text-slate-300">
        <div class="max-w-7xl mx-auto py-8 px-4 sm:px-6 lg:px-8 text-center text-sm">
            <p>Aplicação web interativa baseada no artigo "Conexões entre a Educação Crítica e o Uso da Inteligência Artificial para o Desenvolvimento do Letramento Estatístico nos Anos Iniciais".</p>
            <p class="mt-2">Desenvolvido como uma ferramenta de visualização e exploração do conhecimento acadêmico.</p>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            // Smooth scrolling for navigation
            document.querySelectorAll('a[href^="#"]').forEach(anchor => {
                anchor.addEventListener('click', function (e) {
                    e.preventDefault();
                    document.querySelector(this.getAttribute('href')).scrollIntoView({
                        behavior: 'smooth'
                    });
                });
            });

            // Chart.js - Gráfico de Gravidez na Adolescência
            const ctx = document.getElementById('gravidezChart').getContext('2d');
            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Norte', 'Nordeste', 'Brasil', 'Centro-Oeste', 'Sudeste', 'Sul'],
                    datasets: [{
                        label: '% de Nascidos Vivos de Mães Adolescentes (2018)',
                        data: [21.03, 18.15, 14.94, 14.10, 12.21, 12.10],
                        backgroundColor: [
                            'rgba(251, 191, 36, 0.6)', // amber-400
                            'rgba(251, 191, 36, 0.6)',
                            'rgba(67, 56, 202, 0.6)', // indigo-700
                            'rgba(251, 191, 36, 0.6)',
                            'rgba(251, 191, 36, 0.6)',
                            'rgba(251, 191, 36, 0.6)'
                        ],
                        borderColor: [
                            'rgba(251, 191, 36, 1)',
                            'rgba(251, 191, 36, 1)',
                            'rgba(67, 56, 202, 1)',
                            'rgba(251, 191, 36, 1)',
                            'rgba(251, 191, 36, 1)',
                            'rgba(251, 191, 36, 1)'
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    indexAxis: 'y',
                    scales: {
                        x: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: 'Porcentagem (%)'
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return context.dataset.label + ': ' + context.raw.toFixed(2) + '%';
                                }
                            }
                        }
                    }
                }
            });
        });

        // Function to toggle hidden content
        function toggleContent(id) {
            const element = document.getElementById(id);
            element.classList.toggle('hidden-content');
            element.classList.toggle('visible-content');
        }

        // Tabs functionality
        function openTab(event, tabId) {
            let i, tabcontent, tablinks;
            tabcontent = document.getElementsByClassName("tab-content");
            for (i = 0; i < tabcontent.length; i++) {
                tabcontent[i].style.display = "none";
            }
            tablinks = document.getElementsByClassName("tab-btn");
            for (i = 0; i < tablinks.length; i++) {
                tablinks[i].className = tablinks[i].className.replace(" active", "");
            }
            document.getElementById(tabId).style.display = "block";
            event.currentTarget.className += " active";
        }
        
        // Set the default tab to be open
        document.getElementById("tab-caso-1").style.display = "block";

    </script>
</body>
</html>
