---
slug: /knowledge-base/using-streamlit/pydeck-chart-custom-mapbox-styles
title: How can I make st.pydeck_chart use custom Mapbox styles?
---

# 如何让st.pydeck_chart使用自定义的Mapbox样式？



如果您提供了一个Mapbox令牌，但生成的`pydeck_chart`不显示您自定义的Mapbox样式，请检查是否将Mapbox令牌添加到Streamlit的`config.toml`配置文件中。Streamlit不会从PyDeck规范（即Streamlit应用程序内部）中读取Mapbox令牌。请参阅[论坛帖子](https://discuss.streamlit.io/t/deprecation-warning-deckgl-pydeck-maps-to-require-mapbox-token-for-production-usage/2982/10)获取更多信息。