# Dicionário de Dados

Este documento descreve os arquivos CSV fornecidos no desafio e o significado esperado de cada coluna.

## Observações Gerais

- Os dados representam conteúdo de Instagram e TikTok em nível de post ou story, além de métricas e comentários enriquecidos com sentimento.
- O identificador do conteúdo não é padronizado entre plataformas. No Instagram, o campo de chave é `id`. No TikTok, o campo de chave é `item_id`.
- Os arquivos de comentários usam `post_id` para se relacionar com o conteúdo.
- Os valores de `predicted_sentiment` podem exigir padronização adicional durante a modelagem.
- Datas e timestamps podem exigir tratamento antes do uso analítico.
- O recorte do desafio considera apenas dados no período de `2025-03-01` até `2026-03-31`.
- No caso do Instagram, os arquivos também usam uma seleção específica de contas para manter o volume compatível com o desafio.
- Os nomes de conta nos arquivos finais podem estar pseudonimizados para uso no desafio.

## `instagram_media.csv`

Conteúdo de Instagram em nível de publicação, combinando posts de feed e stories.

| Coluna | Tipo esperado | Descrição |
|---|---|---|
| `content_source` | string | Origem do conteúdo no Instagram. Valores esperados: `media` ou `story`. |
| `id` | string | Identificador do conteúdo no Instagram. Chave para relacionamento com `instagram_media_insights.csv` e `instagram_comments.csv`. |
| `username` | string | Nome pseudonimizado da conta autora do conteúdo. |
| `timestamp` | timestamp | Momento de publicação do conteúdo. |
| `like_count` | integer | Quantidade de curtidas observada no conteúdo. Em stories, pode ser `0` ou pouco relevante. |
| `media_type` | string | Tipo principal de mídia. Exemplos esperados: `IMAGE`, `VIDEO`, `CAROUSEL_ALBUM`. |
| `comments_count` | integer | Quantidade de comentários do conteúdo. Pode ser `NULL` para stories. |
| `is_comment_enabled` | boolean | Indica se comentários estavam habilitados. Pode ser `NULL` para stories. |
| `media_product_type` | string | Tipo de produto do conteúdo na plataforma. Exemplos esperados: `FEED`, `STORY`. |

## `instagram_media_insights.csv`

Métricas de performance de conteúdo de Instagram, combinando posts de feed e stories.

| Coluna | Tipo esperado | Descrição |
|---|---|---|
| `content_source` | string | Origem do conteúdo no Instagram. Valores esperados: `media` ou `story`. |
| `id` | string | Identificador do conteúdo no Instagram. Chave para relacionamento com `instagram_media.csv`. |
| `likes` | integer | Quantidade de curtidas observada nas métricas. Pode ser `NULL` para stories. |
| `reach` | integer | Alcance do conteúdo. |
| `saved` | integer | Quantidade de salvamentos. Pode ser `NULL` para stories. |
| `views` | integer | Quantidade de visualizações. |
| `shares` | integer | Quantidade de compartilhamentos. |
| `follows` | integer | Quantidade de seguidores gerados pelo conteúdo, quando disponível. |
| `comments` | integer | Quantidade de comentários medida na camada de insights. Pode ser `NULL` para stories. |
| `replies` | integer | Quantidade de respostas. Pode ser `NULL` para posts de feed. |
| `profile_visits` | integer | Quantidade de visitas ao perfil atribuídas ao conteúdo. |
| `total_interactions` | integer | Total de interações, quando disponível na origem. |

## `instagram_comments.csv`

Comentários de conteúdos de Instagram com classificação de sentimento.

| Coluna | Tipo esperado | Descrição |
|---|---|---|
| `social_media` | string | Plataforma do comentário. Valor esperado: `instagram`. |
| `post_id` | string | Identificador do conteúdo comentado. Relaciona com `instagram_media.csv.id`. |
| `comment_id` | string | Identificador do comentário. |
| `comment_timestamp` | timestamp | Momento em que o comentário foi publicado. |
| `predicted_sentiment` | string | Sentimento previsto para o comentário. Exemplos esperados: `positivo`, `neutro`, `negativo`. |
| `confidence_sentiment` | float | Grau de confiança associado à classificação de sentimento. |

## `tiktok_posts.csv`

Conteúdo de TikTok em nível de post.

| Coluna | Tipo esperado | Descrição |
|---|---|---|
| `item_id` | string | Identificador do conteúdo no TikTok. Chave para relacionamento com `tiktok_comments.csv`. |
| `create_time` | string | Momento de criação do conteúdo na origem. Pode exigir conversão de tipo antes do uso analítico. |
| `business_username` | string | Nome pseudonimizado da conta autora do conteúdo. |
| `likes` | integer | Quantidade de curtidas. |
| `comments` | integer | Quantidade de comentários. |
| `shares` | integer | Quantidade de compartilhamentos. |
| `video_views` | integer | Quantidade de visualizações do vídeo. |
| `reach` | integer | Alcance do conteúdo. |
| `favorites` | integer | Quantidade de favoritos. |
| `profile_views` | integer | Quantidade de visitas ao perfil atribuídas ao conteúdo. |
| `new_followers` | integer | Quantidade de novos seguidores atribuídos ao conteúdo. |
| `total_time_watched` | integer | Tempo total assistido, quando disponível. |
| `average_time_watched` | float | Tempo médio assistido. |
| `full_video_watched_rate` | float | Taxa de visualização completa do vídeo. |
| `video_duration` | float | Duração do vídeo. |
| `app_download_clicks` | integer | Cliques para download do app, quando disponível. |
| `lead_submissions` | integer | Conversões ou envios de lead, quando disponível. |
| `phone_number_clicks` | integer | Cliques em número de telefone, quando disponível. |
| `website_clicks` | integer | Cliques em website, quando disponível. |

## `tiktok_comments.csv`

Comentários de conteúdos de TikTok com classificação de sentimento.

| Coluna | Tipo esperado | Descrição |
|---|---|---|
| `social_media` | string | Plataforma do comentário. Valor esperado: `tiktok`. |
| `post_id` | string | Identificador do conteúdo comentado. Relaciona com `tiktok_posts.csv.item_id`. |
| `comment_id` | string | Identificador do comentário. |
| `comment_timestamp` | timestamp | Momento em que o comentário foi publicado. |
| `predicted_sentiment` | string | Sentimento previsto para o comentário. Exemplos esperados: `positivo`, `neutro`, `negativo`. |
| `confidence_sentiment` | float | Grau de confiança associado à classificação de sentimento. |

## Relacionamentos Esperados

- `instagram_media.csv.id = instagram_media_insights.csv.id`
- `instagram_media.csv.id = instagram_comments.csv.post_id`
- `tiktok_posts.csv.item_id = tiktok_comments.csv.post_id`

## Observações de Modelagem

- O mesmo conceito de conteúdo aparece com nomes de chave diferentes entre plataformas.
- Stories e posts de feed compartilham o mesmo CSV de conteúdo de Instagram, mas nem todas as métricas estão disponíveis para ambos.
- Alguns campos podem vir nulos por limitação da fonte ou por não se aplicarem ao tipo de conteúdo.
- A harmonização entre métricas de Instagram e TikTok faz parte do desafio.