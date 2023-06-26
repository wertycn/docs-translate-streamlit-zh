---
title: 如何使st.pydeck_chart使用自定义的Mapbox样式？
slug: /knowledge-base/using-streamlit/pydeck-chart-custom-mapbox-styles
---

# 如何使st.pydeck_chart使用自定义的Mapbox样式？

如果您提供了Mapbox令牌，但生成的`pydeck_chart`不显示您的自定义Mapbox样式，请确保将Mapbox令牌添加到Streamlit的`config.toml`配置文件中。Streamlit不会从PyDeck规范（即从Streamlit应用程序内部）读取Mapbox令牌。请参阅[论坛帖子](https://discuss.streamlit.io/t/deprecation-warning-deckgl-pydeck-maps-to-require-mapbox-token-for-production-usage/2982/10)获取更多信息。
