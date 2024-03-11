ckedit是好用的文字編輯器

js檔可以用線上build網址，選擇自己需要的plugin  
https://ckeditor.com/ckeditor-5/online-builder/

_edit.blade.php_
``` blade
<x-app-layout>
  <div class="mb-3">
    <label for="{{ $key }}" class="form-label">
      {{ $key }}
      @if($element['required'])<span class="text-danger">*</span> @endif
    </label>
    <textarea id="{{ $key }}" name="{{ $key }}" class="html-editor" data-title="{{ $key }}">{{ $content[$key] ?? '' }}</textarea>
  </div>

  <!-- footer 額外script -->
  <x-slot name="footer">
    <script src="{{ asset('js/libraries/ckeditor.js') }}" defer></script>
    <!-- todo 可改名content為block,符合service與controller -->
    <script src="{{ asset('js/edit.js') }}"></script>
  </x-slot>
</x-app-layout>
```
_edit.js_
```edit.js
document.querySelectorAll('.html-editor').forEach(function (item) {
  ClassicEditor
    .create(document.querySelector(`#` + item.id), {
      toolbar: {
        items: ['fontsize', 'fontColor', '|', 'link', '|', 'bold', 'italic', 'underline', 'alignment', '|', 'mediaEmbed', 'undo', 'redo']
        // 看官方用法不是這樣，不過跑得動
      },
      link: {
        decorators: {
          isExternal: {
            mode: 'manual',
            label: 'Open in a new tab',
            attributes: {
              target: '_blank'
            }
          }
        }
      }
      , mediaEmbed:
      {
        previewsInData: true
      }
    })
    .then(editor => {
      window.editor = editor;
    })
    .catch(error => {
      console.error(error);
    });
});

document.querySelector('#btn-submit').addEventListener('click', () => {
  console.log(editor.getData());
}
```
