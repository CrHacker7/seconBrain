 ---
title: Example Title
draft: false
tags:
  - example-tag
---
 
The rest of your content lives here. You can use **Markdown** here :)
 
 Documentación para el deploy y más https://quartz.jzhao.xyz/ 
 
1. clonar repo quartz
2. mv quartz secondBrain
3. cd secondBrain
4. npm i
5. npx quartz create
6. empty quartz
7. treat links as shortest path
8. crear repo en github público, sin readme ni license
9. git remote add origin https://SecondBrain (aviso: ya existe)
10. git remote rm origin
11. git remote add origin https://SeconBrain
12. git remote -v (origin:secondBrain - upstream:quartz)
13. npx quartz sync --no-pull
14. git config username y name
15. añadir mis notas md en el folder content
16. npx quartz sync (lo sube automáticamente a github)
17. npx quartz build --serve (nos da el link de visualización en local)
18. crear .github/workflows/deploy.yml (doc 6. host)
19. ir al remoto -> settings -> pages -> source:githubActions
20. npx quartz sync (deploy site)
