# La Tienda de Esme

Lo que la Tienda publica: **ficheros firmados, nada más**. Esme no los confía porque
vengan de aquí, sino porque la firma es válida con la clave que la app tiene fijada.

| fichero | qué es | quién lo firma |
|---|---|---|
| `catalogo.json` | los productos de la tienda (pieles, herramientas, agentes, apps) | la llave de release de Esme |
| `veredictos-skills.json` | el veredicto del juez de las skills: qué entra, con qué riesgo y por qué | la llave del juez |
| `panel.md` | el mismo veredicto, legible por una persona | — |

Nada de esto es un servidor: son ficheros estáticos que la app baja por HTTPS y
verifica antes de creerse nada. Lo que no trae una firma válida no se usa ni se
guarda.

## De dónde sale el veredicto de las skills

Lo dicta el juez del kit (`tienda/skills.ts`), con hechos del catálogo y nada más:
commit fijado, huella del árbol, licencia que deja redistribuir, scripts ejecutables
y peso. La firma cubre **la huella del árbol que se juzgó** y **el catálogo exacto
que se juzgó**, así que:

- si un árbol cambia después del juicio, la app no lo instala;
- si el catálogo de la app cambia, el veredicto deja de aplicar y las skills vuelven
  a «sin medir». El juez pone el listón, nunca lo quita.

El formato firmado es una spec abierta (`estandar/skill-veredicto.spec.yaml` en el
kit de Esme): quien produce y quien verifica sacan los mismos bytes porque el orden
de las claves es parte del contrato.

Para regenerarlo y publicarlo:

```bash
npm run skills:publicar -- --repo /ruta/al/clone --push
```

Las llaves de release nunca viven en un repo: la de la tienda y la del juez se
inyectan por entorno (`ESME_PRIV_KEY`).
