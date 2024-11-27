<script setup>
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'
</script>

<template>
  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #heading>Règles</template>
    <p>Voici les<RouterLink to="/about" rel="noopener">règles officielles</RouterLink>du 
      triman !</p>
  </WelcomeItem>
</template>
<style scoped>
p {
  letter-spacing: .7px;
  
}
a {
  color: var(--blue);
}
</style>