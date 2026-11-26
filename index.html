/* =====================================================================
   ROYALE SENAI — melhorias.js
   Gráficos, física/efeitos das armas e mapa (somente no cliente).
   Como usar: coloque este arquivo na mesma pasta do jogo e adicione,
   logo ANTES de </body> e DEPOIS do <script> principal:
       <script src="melhorias.js"></script>
   ===================================================================== */
(function () {
  'use strict';
  if (typeof THREE === 'undefined' || typeof renderer === 'undefined' || typeof scene === 'undefined') {
    console.warn('melhorias.js: carregue este arquivo depois do script principal do jogo.');
    return;
  }
  const T = THREE;
  const rnd = (a, b) => a + Math.random() * (b - a);
  const SHADOWS = !isTouch;               // sombras só no computador (celular fica leve)
  const mulberry = a => () => {
    a |= 0; a = a + 0x6D2B79F5 | 0;
    let t = Math.imul(a ^ a >>> 15, 1 | a);
    t = t + Math.imul(t ^ t >>> 7, 61 | t) ^ t;
    return ((t ^ t >>> 14) >>> 0) / 4294967296;
  };

  /* ---------------------------------------------------------------
     1) RENDERER, LUZ E SOMBRAS
     --------------------------------------------------------------- */
  renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, isTouch ? 1.5 : 2));
  renderer.setSize(innerWidth, innerHeight);
  camera.far = 1600;
  camera.updateProjectionMatrix();
  scene.fog.far = 430;

  scene.children.forEach(o => {
    if (o.isHemisphereLight) { o.intensity = .8; o.color.set(0xeaf4ff); o.groundColor.set(0x4d6a3a); }
  });
  sun.color.set(0xfff0d0);
  sun.intensity = 1.05;

  if (SHADOWS) {
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = T.PCFSoftShadowMap;
    sun.castShadow = true;
    sun.shadow.mapSize.set(2048, 2048);
    const sc = sun.shadow.camera;
    sc.left = -75; sc.right = 75; sc.top = 75; sc.bottom = -75; sc.near = 10; sc.far = 320;
    sun.shadow.bias = -0.0005;
    sun.shadow.normalBias = 0.03;
    scene.add(sun.target);
    ground.receiveShadow = true;
    scene.children.forEach(o => { if (o.isMesh && o.geometry.type === 'PlaneGeometry') o.receiveShadow = true; });
  }

  // luz dinâmica usada em disparos e explosões
  const fxL = new T.PointLight(0xffb060, 0, 18, 2);
  fxL.position.set(0, -50, 0);
  scene.add(fxL);
  let fxRate = 25;
  function kickLight(col, inten, dist, rate, x, y, z) {
    fxL.color.set(col); fxL.intensity = inten; fxL.distance = dist; fxRate = rate;
    fxL.position.set(x, y, z);
  }

  /* ---------------------------------------------------------------
     2) CÉU: sol, nuvens, montanhas distantes, chão e floresta além da borda
     --------------------------------------------------------------- */
  function softTex(inner, outer) {
    const c = document.createElement('canvas'); c.width = c.height = 64;
    const x = c.getContext('2d');
    const g = x.createRadialGradient(32, 32, 0, 32, 32, 32);
    g.addColorStop(0, inner); g.addColorStop(1, outer);
    x.fillStyle = g; x.fillRect(0, 0, 64, 64);
    return new T.CanvasTexture(c);
  }
  const smokeTex = softTex('rgba(215,215,215,.95)', 'rgba(215,215,215,0)');
  const flashTex = softTex('rgba(255,244,190,1)', 'rgba(255,150,40,0)');
  const cloudTex = softTex('rgba(255,255,255,.95)', 'rgba(255,255,255,0)');
  const sunTex = softTex('rgba(255,252,225,1)', 'rgba(255,215,140,0)');

  const SUNDIR = new T.Vector3(60, 100, 40).normalize();
  const sunSpr = new T.Sprite(new T.SpriteMaterial({ map: sunTex, transparent: true, depthWrite: false, fog: false, blending: T.AdditiveBlending }));
  sunSpr.scale.set(110, 110, 1);
  scene.add(sunSpr);

  const clouds = new T.Group();
  for (let i = 0; i < 18; i++) {
    const a = Math.random() * 6.283, R = rnd(240, 420);
    const s = new T.Sprite(new T.SpriteMaterial({ map: cloudTex, transparent: true, opacity: rnd(.4, .75), depthWrite: false, fog: false }));
    const w = rnd(90, 190);
    s.scale.set(w, w * rnd(.28, .42), 1);
    s.position.set(Math.cos(a) * R, rnd(130, 240), Math.sin(a) * R);
    clouds.add(s);
  }
  scene.add(clouds);

  // chão distante (some o "vazio" além do mapa)
  const outer = new T.Mesh(new T.PlaneGeometry(5000, 5000), new T.MeshLambertMaterial({ color: 0x4a8a3c }));
  outer.rotation.x = -Math.PI / 2; outer.position.y = -.06;
  scene.add(outer);

  // montanhas com topo nevado
  {
    const rm = mulberry(1234);
    for (let i = 0; i < 40; i++) {
      const a = i / 40 * Math.PI * 2 + rm() * .1, R = 820 + rm() * 160;
      const h = 110 + rm() * 140, r = 130 + rm() * 110;
      const col = new T.Color().setHSL(.58, .28, .5 + rm() * .1);
      const cone = new T.Mesh(new T.ConeGeometry(r, h, 7), new T.MeshLambertMaterial({ color: col, fog: false }));
      cone.position.set(Math.cos(a) * R, h / 2 - 6, Math.sin(a) * R);
      cone.rotation.y = rm() * 3;
      const cap = new T.Mesh(new T.ConeGeometry(r * .36, h * .36, 7), new T.MeshLambertMaterial({ color: 0xf4f8fc, fog: false }));
      cap.position.set(cone.position.x, h * .82 - 6, cone.position.z);
      cap.rotation.y = cone.rotation.y;
      scene.add(cone, cap);
    }
  }

  function inst(geo, mat, arr) {
    const im = new T.InstancedMesh(geo, mat, arr.length);
    const d = new T.Object3D(), c = new T.Color();
    arr.forEach((a, i) => {
      d.position.set(a.x, a.y || 0, a.z);
      d.rotation.set(a.rx || 0, a.ry || 0, a.rz || 0);
      d.scale.set(a.sx || 1, a.sy || 1, a.sz || 1);
      d.updateMatrix();
      im.setMatrixAt(i, d.matrix);
      if (a.c !== undefined) { c.setHex(a.c); im.setColorAt(i, c); }
    });
    im.instanceMatrix.needsUpdate = true;
    if (im.instanceColor) im.instanceColor.needsUpdate = true;
    im.frustumCulled = false;
    return im;
  }
  const hsl = (h, s, l) => new T.Color().setHSL(h, s, l).getHex();

  // floresta de borda (além de x/z = ±455): marca o limite do mapa
  {
    const rm = mulberry(777), trunks = [], tops = [];
    for (let i = 0; i < 700; i++) {
      const x = (rm() - .5) * 1120, z = (rm() - .5) * 1120;
      if (Math.abs(x) < 456 && Math.abs(z) < 456) continue;
      const h = 6 + rm() * 7, ht = h * .45, hc = h * 1.15;
      trunks.push({ x, y: ht / 2, z, sx: 1, sy: ht, sz: 1, c: 0x5a3d22 });
      tops.push({ x, y: ht + hc / 2 - .4, z, sx: h * .32, sy: hc, sz: h * .32, c: hsl(.33 + rm() * .05, .5, .16 + rm() * .1) });
    }
    scene.add(inst(new T.CylinderGeometry(.35, .45, 1, 6), new T.MeshLambertMaterial({ color: 0xffffff }), trunks));
    scene.add(inst(new T.ConeGeometry(1, 1, 7), new T.MeshLambertMaterial({ color: 0xffffff }), tops));
  }

  // marcações da cidade: faixa central e faixas de pedestre (decorativas)
  {
    const cx = CITY[0], cz = CITY[1], ST = [-52, 0, 52], dashes = [], zebra = [];
    const nearX = v => ST.some(s => Math.abs(v - s) < 9);
    for (const st of ST) {
      for (let t = -100; t <= 100; t += 6) {
        if (nearX(t)) continue;
        dashes.push({ x: cx + t, y: .1, z: cz + st, sx: 2.4, sy: 1, sz: 1 });
        dashes.push({ x: cx + st, y: .1, z: cz + t, sx: 1, sy: 1, sz: 2.4 });
      }
    }
    for (const sx of ST) for (const sz of ST) {
      for (const sd of [-1, 1]) {
        for (let k = -2.5; k <= 2.5; k++) {
          zebra.push({ x: cx + sx + sd * 9 + k * 1.9, y: .1, z: cz + sz, sx: 1, sy: 1, sz: 7 });   // atravessa a rua em z
          zebra.push({ x: cx + sx, y: .1, z: cz + sz + sd * 9 + k * 1.9, sx: 7, sy: 1, sz: 1 });   // atravessa a rua em x
        }
      }
    }
    const mk = (col) => { const m = new T.MeshBasicMaterial({ color: col, polygonOffset: true, polygonOffsetFactor: -4, polygonOffsetUnits: -4 }); return m; };
    const g = new T.BoxGeometry(1, .02, 1);
    scene.add(inst(g, mk(0xe8c547), dashes.map(a => Object.assign({}, a, { sx: (a.sx > 1 ? 2.4 : .35), sz: (a.sz > 1 ? 2.4 : .35) }))));
    scene.add(inst(g, mk(0xf2f2f2), zebra));
  }

  /* ---------------------------------------------------------------
     3) DECORAÇÃO POR PARTIDA (sem colisão: não muda a jogabilidade)
     --------------------------------------------------------------- */
  const tuftGeo = new T.ConeGeometry(.12, .55, 4); tuftGeo.translate(0, .27, 0);
  const stemGeo = new T.CylinderGeometry(.014, .014, .36, 4); stemGeo.translate(0, .18, 0);
  const headGeo = new T.SphereGeometry(.09, 6, 4);
  const white = new T.MeshLambertMaterial({ color: 0xffffff });
  const stemMat = new T.MeshLambertMaterial({ color: 0x3f7d33 });

  function decorate(seed) {
    const r = mulberry((seed | 0) * 7919 + 13);
    const tufts = [], stems = [], heads = [];
    let tries = 0;
    while (tufts.length < 2600 && tries++ < 9000) {
      const x = (r() - .5) * 880, z = (r() - .5) * 880;
      if (noTree(x, z) || inSnow(x, z)) continue;
      const s = .7 + r() * 1.1;
      tufts.push({ x, z, ry: r() * 6, sx: s, sy: s * (.8 + r() * .8), sz: s, c: hsl(.22 + r() * .08, .55, .26 + r() * .18) });
    }
    tries = 0;
    const FL = [0xffffff, 0xffe14d, 0xff9ecb, 0xb9a3ff, 0xff6b6b];
    while (heads.length < 700 && tries++ < 6000) {
      const x = (r() - .5) * 880, z = (r() - .5) * 880;
      if (noTree(x, z) || inSnow(x, z)) continue;
      const s = .8 + r() * .6;
      stems.push({ x, z, sx: s, sy: s, sz: s });
      heads.push({ x, y: .36 * s, z, sx: s, sy: s, sz: s, c: FL[(r() * FL.length) | 0] });
    }
    const add = m => world.add(m);
    add(inst(tuftGeo, white, tufts));
    add(inst(stemGeo, stemMat, stems));
    add(inst(headGeo, white, heads));

    // detalhes de telhado nos prédios altos
    for (const o of OB) {
      if (o.tp !== 'bld' || o.k !== 1 || o.hx < 6 || o.h < 8) continue;
      add(box(o.hx * 2 + .5, .5, o.hz * 2 + .5, 0x3b3f45, o.x, o.h + .25, o.z));
      const ax = o.x + (r() - .5) * (o.hx * 2 - 5), az = o.z + (r() - .5) * (o.hz * 2 - 5);
      add(box(2.4, 1.2, 2, 0x9aa0a6, ax, o.h + 1.1, az));
      if (o.h > 18 && r() < .7) {
        add(box(.14, 6, .14, 0x333333, o.x, o.h + 3.5, o.z));
        const l = new T.Mesh(new T.SphereGeometry(.28, 8, 6), new T.MeshBasicMaterial({ color: 0xff3030 }));
        l.position.set(o.x, o.h + 6.6, o.z);
        add(l);
      }
    }
  }

  function applyShadows() {
    if (!SHADOWS || !world) return;
    world.traverse(o => {
      if (!o.isMesh) return;
      const t = o.geometry.type;
      const flat = t === 'PlaneGeometry' || t === 'CircleGeometry' || t === 'RingGeometry' || o.isInstancedMesh;
      o.receiveShadow = true;
      o.castShadow = !flat && !(o.material && o.material.transparent);
    });
  }

  const _build = window.build;
  window.build = function (seed) {
    _build(seed);
    try { decorate(seed); } catch (e) { console.error('melhorias.js decorate', e); }
    applyShadows();
  };

  const _mk = window.mk;
  window.mk = function (nick, skin, noAdd) {
    const g = _mk(nick, skin, noAdd);
    if (SHADOWS) g.traverse(o => { if (o.isMesh) o.castShadow = true; });
    return g;
  };

  /* ---------------------------------------------------------------
     4) ARMAS MAIS DETALHADAS (modelos)
     --------------------------------------------------------------- */
  function gunExtras(g, w) {
    const D = 0x1d2025, M = 0x3a3f48, W = 0xf2f2f2;
    const add = (...a) => a.forEach(m => g.add(m));
    if (w === 0) {
      for (let i = 0; i < 4; i++) add(box(.066, .03, .012, D, 0, .075, .02 + i * .026));
      add(box(.012, .06, .012, D, 0, -.045, -.02), box(.012, .012, .07, D, 0, -.075, .015),
          box(.05, .02, .1, M, 0, -.01, -.15), box(.012, .02, .012, W, 0, .1, -.19));
    } else if (w === 1) {
      add(box(.05, .06, .16, D, 0, .13, -.08), cyl(.025, .16, D, 0, .15, -.08),
          cyl(.026, .01, 0x66ccff, 0, .15, -.165), box(.04, .015, .4, M, 0, .065, -.1),
          box(.03, .03, .05, D, .045, .04, .05), box(.05, .03, .2, D, 0, .06, .28));
    } else if (w === 2) {
      for (let i = 0; i < 3; i++) add(cyl(.017, .05, 0xcc2a2a, .052, -.005, .02 - i * .04));
      for (let i = 0; i < 3; i++) add(box(.092, .012, .02, D, 0, -.012, -.36 + i * .06));
      add(box(.01, .02, .01, 0xffee88, 0, .04, -.8));
    } else if (w === 3) {
      add(box(.05, .05, .08, D, 0, .01, -.9), box(.1, .02, .02, D, .07, .04, .08), box(.03, .03, .03, M, .12, .04, .08));
      const b1 = box(.012, .2, .012, D, -.05, -.1, -.62); b1.rotation.z = .35;
      const b2 = box(.012, .2, .012, D, .05, -.1, -.62); b2.rotation.z = -.35;
      add(b1, b2);
    } else if (w === 4) {
      add(box(.03, .1, .02, D, 0, .14, .1), box(.015, .08, .015, D, 0, .13, -.75),
          cyl(.098, .03, 0xffcc33, 0, .02, -.55), cyl(.098, .03, 0xffcc33, 0, .02, -.1),
          box(.04, .14, .05, D, 0, -.11, -.4));
    }
  }
  const _makeGun = window.makeGun;
  window.makeGun = function (w, c, full) {
    const g = _makeGun(w, c, full);
    try { gunExtras(g, w); } catch (e) { }
    return g;
  };

  /* ---------------------------------------------------------------
     5) FÍSICA E EFEITOS DOS TIROS
     --------------------------------------------------------------- */
  // fumaça
  const smokes = [];
  function puff(x, y, z, vx, vy, vz, s0, s1, life, op, col) {
    const m = new T.SpriteMaterial({ map: smokeTex, transparent: true, depthWrite: false, opacity: op, color: col || 0xffffff });
    const s = new T.Sprite(m);
    s.position.set(x, y, z); s.scale.setScalar(s0); scene.add(s);
    smokes.push({ s, vx, vy, vz, s0, s1, life, t: 0, op });
    if (smokes.length > 150) { const o = smokes.shift(); scene.remove(o.s); o.s.material.dispose(); }
  }
  function updSmoke(dt) {
    for (let i = smokes.length - 1; i >= 0; i--) {
      const p = smokes[i];
      p.t += dt;
      const a = p.t / p.life;
      if (a >= 1) { scene.remove(p.s); p.s.material.dispose(); smokes.splice(i, 1); continue; }
      p.s.position.x += p.vx * dt; p.s.position.y += p.vy * dt; p.s.position.z += p.vz * dt;
      p.vx *= .97; p.vz *= .97; p.vy *= .985;
      p.s.scale.setScalar(p.s0 + (p.s1 - p.s0) * a);
      p.s.material.opacity = p.op * (1 - a) * (1 - a);
    }
  }

  // clarão do cano
  const flashes = [];
  const _muzzle = window.muzzle;
  window.muzzle = function (x, y, z, w) {
    _muzzle(x, y, z, w);
    const s = new T.Sprite(new T.SpriteMaterial({ map: flashTex, blending: T.AdditiveBlending, transparent: true, depthWrite: false }));
    s.position.set(x, y, z);
    s.scale.setScalar([.9, 1.1, 1.9, 1.5, 2.8][w] || 1);
    s.material.rotation = Math.random() * 6.28;
    scene.add(s);
    flashes.push({ s, t: 0, life: .07 });
    kickLight(0xffb060, w === 4 ? 4 : 2.4, 16, 28, x, y, z);
  };
  function updFlashes(dt) {
    for (let i = flashes.length - 1; i >= 0; i--) {
      const f = flashes[i];
      f.t += dt;
      if (f.t >= f.life) { scene.remove(f.s); f.s.material.dispose(); flashes.splice(i, 1); continue; }
      f.s.material.opacity = 1 - f.t / f.life;
    }
  }

  // cápsulas ejetadas (com gravidade e quique)
  const casings = [];
  const cGeo = new T.BoxGeometry(.03, .03, .08), cMat = new T.MeshLambertMaterial({ color: 0xd9b44a });
  const sGeo = new T.CylinderGeometry(.022, .022, .06, 8), sMat = new T.MeshLambertMaterial({ color: 0xc8372d });
  function casing(x, y, z, dx, dz, w) {
    if (w === 4) return;
    const l = Math.hypot(dx, dz) || 1, rx = -dz / l, rz = dx / l;
    const m = new T.Mesh(w === 2 ? sGeo : cGeo, w === 2 ? sMat : cMat);
    m.position.set(x + rx * .1, y, z + rz * .1);
    scene.add(m);
    const sp = rnd(2, 3.5);
    casings.push({ m, vx: rx * sp + dx / l * rnd(-.5, .5), vy: rnd(2.5, 4), vz: rz * sp + dz / l * rnd(-.5, .5), sx: rnd(-14, 14), sy: rnd(-14, 14), t: 0 });
    if (casings.length > 40) { const o = casings.shift(); scene.remove(o.m); }
  }
  function updCasings(dt) {
    for (let i = casings.length - 1; i >= 0; i--) {
      const c = casings[i], p = c.m.position;
      c.t += dt;
      if (c.t > 2.6) { scene.remove(c.m); casings.splice(i, 1); continue; }
      c.vy -= 20 * dt;
      p.x += c.vx * dt; p.y += c.vy * dt; p.z += c.vz * dt;
      if (p.y < .03) {
        p.y = .03;
        if (Math.abs(c.vy) > .9) { c.vy *= -.35; c.sx *= .5; c.sy *= .5; } else { c.vy = 0; c.sx = c.sy = 0; }
        c.vx *= .6; c.vz *= .6;
      }
      c.m.rotation.x += c.sx * dt; c.m.rotation.y += c.sy * dt;
      if (c.t > 2) c.m.scale.setScalar(Math.max(.01, 1 - (c.t - 2) / .6));
    }
  }

  // rastros de bala (viajam de verdade em vez de piscar)
  const tracers = [];
  const trMat = new T.MeshBasicMaterial({ color: 0xffd27a, transparent: true, opacity: .9, blending: T.AdditiveBlending, depthWrite: false });
  const trSn = new T.MeshBasicMaterial({ color: 0x9fe4ff, transparent: true, opacity: .95, blending: T.AdditiveBlending, depthWrite: false });
  const trGeo = new T.BoxGeometry(.035, .035, 2.2);
  function tracer(ox, oy, oz, d, w) {
    if (w === 4) return;
    const n = w === 2 ? 5 : 1;
    for (let i = 0; i < n; i++) {
      const sp = w === 2 ? .06 : 0;
      const v = new T.Vector3(d[0] + rnd(-sp, sp), d[1] + rnd(-sp, sp), d[2] + rnd(-sp, sp)).normalize();
      const m = new T.Mesh(trGeo, w === 3 ? trSn : trMat);
      m.position.set(ox, oy, oz);
      m.lookAt(ox + v.x, oy + v.y, oz + v.z);
      if (w === 3) m.scale.set(1.6, 1.6, 1.6);
      scene.add(m);
      tracers.push({ m, v, sp: w === 3 ? 700 : w === 1 ? 430 : w === 2 ? 260 : 360, d: 0 });
    }
    while (tracers.length > 60) scene.remove(tracers.shift().m);
  }
  function updTracers(dt) {
    for (let i = tracers.length - 1; i >= 0; i--) {
      const t = tracers[i], s = t.sp * dt;
      t.m.position.addScaledVector(t.v, s);
      t.d += s;
      if (t.d > 90) { scene.remove(t.m); tracers.splice(i, 1); }
    }
  }

  // recuo da câmera e tremor
  let pP = 0, pY = 0, shake = 0;
  const KICK = [.018, .007, .06, .09, .08];
  const _v = new T.Vector3();

  function mineShot(m) {
    if (!myAv || !m.d) return;
    const w = m.w | 0, d = m.d;
    myAv.g.userData.hand.getWorldPosition(_v);
    tracer(_v.x + d[0] * 1.2, _v.y + d[1] * 1.2 + .05, _v.z + d[2] * 1.2, d, w);
    casing(_v.x, _v.y + .05, _v.z, d[0], d[2], w);
    puff(_v.x + d[0] * 1.3, _v.y + d[1] * 1.3, _v.z + d[2] * 1.3, d[0] * .6, .5, d[2] * .6, .25, w === 3 || w === 2 ? 1.4 : .8, .7, .35);
    if (w === 4) for (let i = 0; i < 4; i++) puff(_v.x - d[0] * (1 + i * .5), _v.y, _v.z - d[2] * (1 + i * .5), rnd(-1, 1) - d[0] * 2, rnd(.2, 1), rnd(-1, 1) - d[2] * 2, .6, 2.6, 1.1, .5);
    const k = KICK[w] || .01, z = Math.max(.5, camera.fov / 75);
    pP = Math.min(.2, pP + k * z);
    pY += rnd(-1, 1) * k * .45 * z;
  }
  const _tx = window.tx;
  window.tx = function (m) {
    if (m && m.t === 'shoot') { try { mineShot(m); } catch (e) { } }
    return _tx(m);
  };

  const _onShot = window.onShot;
  window.onShot = function (m) {
    _onShot(m);
    if (m.id === myId || !m.d) return;
    if (Math.hypot(m.x - me.x, m.z - me.z) > 120) return;
    const d = m.d, w = m.w | 0, y = m.y + 1.5;
    tracer(m.x + d[0] * 1.3, y + d[1] * 1.3, m.z + d[2] * 1.3, d, w);
    casing(m.x, y - .1, m.z, d[0], d[2], w);
    puff(m.x + d[0] * 1.3, y + d[1] * 1.3, m.z + d[2] * 1.3, d[0] * .6, .5, d[2] * .6, .25, .9, .7, .3);
  };

  // explosões maiores, com onda de choque, fumaça e tremor
  const rings = [];
  const _exFx = window.exFx;
  window.exFx = function (m) {
    _exFx(m);
    const ring = new T.Mesh(new T.RingGeometry(.8, 1, 28), new T.MeshBasicMaterial({ color: 0xffa040, transparent: true, opacity: .8, side: T.DoubleSide, blending: T.AdditiveBlending, depthWrite: false }));
    ring.rotation.x = -Math.PI / 2; ring.position.set(m.x, Math.max(.15, m.y), m.z);
    scene.add(ring); rings.push({ ring, t: 0 });
    for (let i = 0; i < 14; i++) puff(m.x + rnd(-1, 1), m.y + rnd(0, 1), m.z + rnd(-1, 1), rnd(-3, 3), rnd(1, 4), rnd(-3, 3), 1.5, rnd(6, 10), rnd(1.4, 2.4), .6, i % 3 ? 0x888888 : 0x333333);
    kickLight(0xff9a40, 8, 60, 6, m.x, m.y + 1.5, m.z);
    const d = Math.hypot(m.x - me.x, m.z - me.z);
    if (d < 130) shake = Math.max(shake, .07 * (1 - d / 130));
  };
  function updRings(dt) {
    for (let i = rings.length - 1; i >= 0; i--) {
      const q = rings[i]; q.t += dt;
      const a = q.t / .45;
      if (a >= 1) { scene.remove(q.ring); q.ring.geometry.dispose(); q.ring.material.dispose(); rings.splice(i, 1); continue; }
      q.ring.scale.setScalar(1 + a * 13);
      q.ring.material.opacity = .8 * (1 - a);
    }
  }

  // poeira ao acertar árvores/pedras/prédios
  const _obFx = window.obFx;
  window.obFx = function (m) {
    _obFx(m);
    if (Math.hypot(m.x - me.x, m.z - me.z) > 90) return;
    const col = m.k === 'tree' ? 0x8a6a45 : 0xaaaaaa;
    puff(m.x, m.y, m.z, rnd(-.5, .5), rnd(.3, 1), rnd(-.5, .5), .3, m.b ? 2.4 : 1.1, m.b ? 1.2 : .6, m.b ? .6 : .4, col);
  };

  // tremor ao levar dano
  const _flash = window.flash;
  window.flash = function (d) {
    _flash(d);
    shake = Math.max(shake, Math.min(.05, .012 + (d > 0 ? d : 10) / 2000));
  };

  /* ---------------------------------------------------------------
     6) NEVE CAINDO + ÁRVORES BALANÇANDO + GANCHO DE RENDERIZAÇÃO
     --------------------------------------------------------------- */
  const SN = 700;
  const snowArr = new Float32Array(SN * 3);
  for (let i = 0; i < SN; i++) { snowArr[i * 3] = rnd(-40, 40); snowArr[i * 3 + 1] = rnd(0, 30); snowArr[i * 3 + 2] = rnd(-40, 40); }
  const snowGeo = new T.BufferGeometry();
  snowGeo.setAttribute('position', new T.BufferAttribute(snowArr, 3));
  const snowP = new T.Points(snowGeo, new T.PointsMaterial({ color: 0xffffff, size: .3, transparent: true, opacity: .85, depthWrite: false, fog: false }));
  snowP.frustumCulled = false; snowP.visible = false;
  scene.add(snowP);

  let lastT = performance.now(), trailT = 0;
  function pre() {
    const n = performance.now();
    const dt = Math.min(.05, (n - lastT) / 1000);
    lastT = n;
    const t = n / 1000;

    if (SHADOWS) {
      sun.target.position.set(me.x, 0, me.z);
      sun.position.set(me.x + 60, 100, me.z + 40);
      sun.target.updateMatrixWorld();
    }
    sunSpr.position.copy(camera.position).addScaledVector(SUNDIR, 430);
    clouds.position.set(camera.position.x, 0, camera.position.z);
    clouds.rotation.y = t * .004;

    for (const o of OB) if (o.tp === 'tree' && o.g && !o.fx) o.g.rotation.z = Math.sin(t * 1.4 + o.x * .07 + o.z * .05) * .014;

    // fumaça atrás dos foguetes
    trailT += dt;
    if (trailT > .03) {
      trailT = 0;
      for (const g of projM.values()) puff(g.position.x, g.position.y, g.position.z, rnd(-.2, .2), rnd(0, .3), rnd(-.2, .2), .5, 2.2, .9, .5, 0xcccccc);
    }

    // neve
    const inS = (phase === 'air' || phase === 'ground' || phase === 'spectate') && inSnow(me.x, me.z) && me.y < 45;
    snowP.visible = inS;
    if (inS) {
      const cx = camera.position.x, cz = camera.position.z;
      for (let i = 0; i < SN; i++) {
        const k = i * 3;
        snowArr[k + 1] -= (2.2 + (i % 5) * .3) * dt;
        snowArr[k] += Math.sin(t + i) * .3 * dt;
        if (snowArr[k + 1] < 0) { snowArr[k + 1] = 30; snowArr[k] = cx + rnd(-40, 40); snowArr[k + 2] = cz + rnd(-40, 40); }
        if (snowArr[k] - cx > 40) snowArr[k] -= 80; else if (snowArr[k] - cx < -40) snowArr[k] += 80;
        if (snowArr[k + 2] - cz > 40) snowArr[k + 2] -= 80; else if (snowArr[k + 2] - cz < -40) snowArr[k + 2] += 80;
      }
      snowGeo.attributes.position.needsUpdate = true;
    }

    updSmoke(dt); updFlashes(dt); updCasings(dt); updTracers(dt); updRings(dt);
    fxL.intensity *= Math.exp(-dt * fxRate);
    if (fxL.intensity < .02) { fxL.intensity = 0; fxL.position.y = -50; }

    // recuo e tremor aplicados na câmera deste quadro
    const kd = Math.exp(-dt * 10);
    pP *= kd; pY *= kd; shake *= Math.exp(-dt * 7);
    camera.rotation.x += pP + Math.sin(t * 63) * shake;
    camera.rotation.y += pY + Math.sin(t * 71 + 2) * shake;
    camera.position.y += Math.sin(t * 57 + 1) * shake * 2;
  }

  const _render = renderer.render.bind(renderer);
  let warned = false;
  renderer.render = function (sc, cam) {
    if (sc === scene && cam === camera) {
      try { pre(); } catch (e) { if (!warned) { warned = true; console.error('melhorias.js', e); } }
    }
    _render(sc, cam);
  };

  console.log('melhorias.js carregado');
})();
