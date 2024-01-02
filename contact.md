---
layout: page
permalink: /contact/
title: Fale comigo!
---
<div class="page-wrapper">
  <form name="contact" method="POST" data-netlify="true" class="basic-grey" netlify-honeypot="maverick">
    <p class="honey">
      <label>
        Don’t fill this out if you’re human: <input name="maverick" />
      </label>
    </p>
    <p>
      <label>
        <span>Nome</span>
        <input type="text" name="name" />
       </label>   
    </p>
    <p>
      <label>
        <span>E-mail</span>
        <input type="email" name="email" />
      </label>
    </p>
    <p>
      <label>
        <span>Assunto</span>
        <select name="subject">
          <option value="question">Gostaria de tirar uma dúvida</option>
          <option value="coaching">Gostaria de saber sobre seus serviços</option>
          <option value="other">Só quero bater um papo</option>
        </select>
       </label>
    </p>
    <p>
      <label>
        <span>Mensagem</span>
        <textarea name="message"></textarea>
      </label>
    </p>
    <p>
      <label>
        <span></span>
        <button class="button" type="submit">Enviar</button>
      </label>
    </p>
  </form>
</div>
