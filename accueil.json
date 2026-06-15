
function updateProgress(scopeId){const scope=document.getElementById(scopeId);if(!scope)return;const fields=scope.querySelectorAll('input,select,textarea');let filled=0;fields.forEach(f=>{if(String(f.value||'').trim()!=='')filled++});const pct=fields.length?Math.round((filled/fields.length)*100):0;let bar=scope.querySelector('.progress-bar'),text=scope.querySelector('.progress-value');if(bar)bar.style.width=pct+'%';if(text)text.textContent=pct+'%'}
document.addEventListener('input',e=>{const f=e.target.closest('[data-progress]');if(f)updateProgress(f.id)});document.addEventListener('change',e=>{const f=e.target.closest('[data-progress]');if(f)updateProgress(f.id)});
function fakeSubmit(m){alert(m||'Votre demande a bien été reçue.')}
function toggleChat(){document.querySelector('.chat-panel').classList.toggle('open')}
function chatChoose(type){const b=document.querySelector('.chat-body');const label=type==='shop'?'Je veux commander un pack':type==='devis'?'Je veux un devis sur mesure':'J’ai une question';b.insertAdjacentHTML('beforeend',`<div class="user-msg">${label}</div>`);if(type==='shop'){b.insertAdjacentHTML('beforeend',`<div class="bot-msg">Parfait ! Je vous dirige vers la boutique : Pack Brandy Express, Pack Ballons DIY ou E-books.</div><div class="chat-actions"><button onclick="location.href='boutique.html'">Aller à la boutique</button><button onclick="location.href='pack-express.html'">Pack Express</button></div>`)}else if(type==='devis'){b.insertAdjacentHTML('beforeend',`<div class="bot-msg">Pour une prestation sur mesure, remplissez la demande de devis. Cela permet de comprendre la date, le lieu et l’ambiance souhaitée.</div><div class="chat-actions"><button onclick="location.href='devis.html'">Remplir le devis</button><button onclick="location.href='prestations.html'">Voir les prestations</button></div>`)}else{b.insertAdjacentHTML('beforeend',`<div class="bot-msg">Vous pouvez consulter la FAQ ou nous contacter directement.</div><div class="chat-actions"><button onclick="location.href='faq.html'">FAQ</button><button onclick="location.href='contact.html'">Contact</button></div>`)}b.scrollTop=b.scrollHeight}
document.addEventListener('DOMContentLoaded',()=>document.querySelectorAll('[data-progress]').forEach(el=>updateProgress(el.id)));



function showModal(title, message){
  const existing=document.querySelector('.modal-overlay');
  if(existing) existing.remove();
  const modal=document.createElement('div');
  modal.className='modal-overlay';
  modal.innerHTML=`<div class="modal-card"><h2>${title}</h2><p>${message}</p><button class="btn" type="button" onclick="this.closest('.modal-overlay').remove()">J’ai compris</button></div>`;
  document.body.appendChild(modal);
}
function showOrderConfirmation(){
  showModal('Commande envoyée', 'Votre demande de commande a bien été prise en compte.\n\nL’équipe Brandy vous enverra un e-mail de confirmation avec le lien de paiement. Votre commande sera définitivement validée uniquement après paiement.\n\nLe délai de préparation et de mise à disposition de 24 à 48h démarre à partir de la validation du paiement.');
}
function showDevisConfirmation(){
  showModal('Demande reçue', 'Votre demande a bien été reçue.\n\nL’équipe Brandy vous recontactera sous 24 à 72h afin d’échanger avec vous, établir un devis et confirmer les modalités de la prestation.');
}
function updateColorFields(select){
  const form=select.closest('form');
  const fields=form?.querySelector('[data-color-fields]');
  if(!fields) return;
  const brandy=select.value.toLowerCase().includes('brandy choisit');
  fields.classList.toggle('disabled', brandy);
  fields.querySelectorAll('input').forEach(input=>{input.disabled=brandy;if(brandy) input.value='';});
}
function updateSupportPreview(select){
  const option=select.selectedOptions[0];
  const preview=document.querySelector('#supportPreview .support-image');
  const label=document.querySelector('#supportPreview p');
  if(!preview || !option) return;
  preview.style.background=option.dataset.image || 'linear-gradient(135deg,#fff,#f8d7c7 38%,#ead4ad 70%,#fff)';
  label.textContent=option.value ? option.value : 'Sélectionnez un support pour visualiser l’option choisie.';
}
function updateBudgetRange(input){
  const value=document.getElementById('budgetValue');
  if(value) value.textContent=Number(input.value).toLocaleString('fr-FR');
}
document.addEventListener('change',e=>{
  if(e.target.matches('[data-color-choice]')) updateColorFields(e.target);
  if(e.target.matches('[data-support-preview]')) updateSupportPreview(e.target);
});
document.addEventListener('input',e=>{
  if(e.target.matches('[data-budget-range]')) updateBudgetRange(e.target);
});
document.addEventListener('DOMContentLoaded',()=>{
  document.querySelectorAll('[data-color-choice]').forEach(updateColorFields);
  document.querySelectorAll('[data-support-preview]').forEach(updateSupportPreview);
  document.querySelectorAll('[data-budget-range]').forEach(updateBudgetRange);
});


async function loadJson(path){
  try{
    const res=await fetch(path,{cache:'no-store'});
    if(!res.ok) return null;
    return await res.json();
  }catch(e){return null;}
}
function youtubeToEmbed(url){
  if(!url) return '';
  if(url.includes('/embed/')) return url;
  const watch=url.match(/[?&]v=([^&]+)/);
  if(watch) return 'https://www.youtube.com/embed/'+watch[1];
  const short=url.match(/youtu\.be\/([^?&]+)/);
  if(short) return 'https://www.youtube.com/embed/'+short[1];
  return url;
}
function serviceIdFromTitle(title){
  return (title||'').toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'').replace(/[^a-z0-9]+/g,'-').replace(/(^-|-$)/g,'');
}
async function hydratePrestations(){
  const data=await loadJson('content/prestations.json');
  if(!data) return;
  const iframe=document.querySelector('[data-cms-youtube]');
  const embed=youtubeToEmbed(data.youtube_url);
  if(iframe && embed) iframe.src=embed;
  const wrap=document.getElementById('prestationsList');
  if(!wrap || !Array.isArray(data.events)) return;
  wrap.innerHTML=data.events.map((event)=>{
    const id=event.slug || serviceIdFromTitle(event.title);
    const gallery=(event.gallery||[]).map((img,i)=>{
      const style=img.image ? `style="background-image:linear-gradient(rgba(0,0,0,.05),rgba(0,0,0,.05)),url('${img.image}');background-size:cover;background-position:center"` : '';
      return `<div class="gallery-item"><div class="gallery-img" ${style}></div><div class="caption">${img.caption||'Ambiance Brandy'}</div></div>`;
    }).join('');
    return `<div class="service-block stacked" id="${id}"><div class="service-copy"><h2>${event.title||''}</h2><p>${event.description||''}</p><ul><li>Direction artistique</li><li>Décor personnalisé</li><li>Installation clé en main</li></ul><a class="btn alt" href="devis.html">Demander un devis</a></div><div class="service-gallery gallery two-rows">${gallery}</div></div>`;
  }).join('');
}
async function hydrateTeam(){
  const data=await loadJson('content/equipe.json');
  const wrap=document.getElementById('teamRow');
  if(!data || !wrap || !Array.isArray(data.members)) return;
  wrap.innerHTML=data.members.map(member=>{
    const style=member.photo ? `style="background-image:url('${member.photo}');background-size:cover;background-position:center"` : '';
    return `<article class="team-card"><div class="team-photo" ${style}></div><h3>${member.name||''}</h3><p class="muted">${member.description||''}</p></article>`;
  }).join('');
}
async function hydrateBoutiquePacks(){
  const data=await loadJson('content/boutique.json');
  if(!data) return;
  document.querySelectorAll('[data-pack-description]').forEach(el=>{
    const pack=(data.packs||[]).find(p=>p.name===el.dataset.packDescription);
    if(pack){ el.textContent=pack.description; }
  });
  const select=document.querySelector('[data-cms-supports]');
  if(select && Array.isArray(data.supports)){
    select.innerHTML='<option value="">Choisir un support</option>'+data.supports.map(s=>`<option value="${s.name||''}" data-image-url="${s.image||''}">${s.name||''}</option>`).join('');
    updateSupportPreview(select);
  }
}
async function hydrateSiteSettings(){
  const data=await loadJson('content/site.json');
  if(!data) return;
  document.querySelectorAll('a[href*="instagram.com/brandyagence.gf"]').forEach(a=>{ if(data.instagram) a.href=data.instagram; });
}
// update support preview with CMS image URL if present
const originalUpdateSupportPreview = typeof updateSupportPreview === 'function' ? updateSupportPreview : null;
updateSupportPreview=function(select){
  const option=select.selectedOptions[0];
  const preview=document.querySelector('#supportPreview .support-image');
  const label=document.querySelector('#supportPreview p');
  if(!preview || !option) return;
  const imageUrl=option.dataset.imageUrl;
  if(imageUrl){ preview.style.background=`url('${imageUrl}') center/cover`; }
  else { preview.style.background=option.dataset.image || 'linear-gradient(135deg,#fff,#f8d7c7 38%,#ead4ad 70%,#fff)'; }
  label.textContent=option.value ? option.value : 'Sélectionnez un support pour visualiser l’option choisie.';
}
document.addEventListener('DOMContentLoaded',()=>{
  hydrateSiteSettings();
  hydratePrestations();
  hydrateTeam();
  hydrateBoutiquePacks();
});
