# Jira-webhook

в локальном репозитории сначала сбилдить образ 

а так на сбм-022 идешь в /store/docker/jira-webhooks - там лежит докер компоуз

и там 

```jsx
docker-compose --pull
docker-compose up -d --build
```